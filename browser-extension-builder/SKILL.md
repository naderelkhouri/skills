---
name: browser-extension-builder
description: Framework e padrão de arquitetura para criação de extensões de navegador modernas (Manifest V3) com suporte universal multi-browser (Vivaldi, Chrome, Edge, Brave, Opera, Arc, Firefox), anti-crash de Service Worker, telemetria local e layout híbrido de painel lateral.
---

# Browser Extension Builder 🚀
**Padrão de Engenharia para Extensões Modernas Manifest V3 Multi-Navegador**

Este guia estabelece o padrão canônico de arquitetura, ciclo de vida e empacotamento para criação de extensões de navegador de alta performance, sem quebras de promessa (*anti-context invalidated*), com telemetria nativa e compatibilidade universal.

---

## 🏗️ 1. Estrutura Canônica de Arquivos

```text
minha-extensao/
├── icons/                  # Ícones em PNG (16x16, 48x48, 128x128)
├── manifest.json           # Manifest V3 para Chromium (Vivaldi, Chrome, Edge, Brave, Opera, Arc)
├── manifest.firefox.json   # Manifest V3 para Firefox / Floorp (com Gecko ID e sidebar_action)
├── background.js           # Service Worker modular com listeners assíncronos e alarmes
├── logger.js               # Telemetria local estruturada com respeito a fuso horário do usuário
├── popup.html              # Interface híbrida responsiva (Pop-up + Painel Lateral + Aba Cheia)
├── popup.js                # Lógica da UI blindada contra context invalidation (safeSendMessage)
├── styles.css              # CSS flexível (height: 100vh, flex: 1, min-height: 0)
└── build.sh                # Script de compilação automatizada (.zip e .xpi)
```

---

## 🛡️ 2. Regra Anti-Crash: Resiliência a Invalidação de Contexto

No Manifest V3, recarregar a extensão ou a hibernação do Service Worker causa o erro fatal:
`Uncaught (in promise) Error: Extension context invalidated`

### Solução Padronizada Obrigatória (`popup.js`):

```javascript
// 1. Interceptar rejeições de promessa não tratadas
window.addEventListener("unhandledrejection", (event) => {
  if (event.reason?.message?.includes("Extension context invalidated")) {
    event.preventDefault(); // Impede tela vermelha no console
    if (updateIntervalId) clearInterval(updateIntervalId);
    showReloadNotice();
  }
});

// 2. Validador de contexto vivo
function isContextValid() {
  try {
    return typeof chrome !== "undefined" && Boolean(chrome?.runtime?.id);
  } catch (e) {
    return false;
  }
}

// 3. Wrapper seguro para troca de mensagens
async function safeSendMessage(payload) {
  if (!isContextValid()) return null;
  return new Promise((resolve) => {
    try {
      chrome.runtime.sendMessage(payload, (response) => {
        if (chrome.runtime.lastError) {
          resolve(null);
          return;
        }
        resolve(response);
      });
    } catch (err) {
      resolve(null);
    }
  });
}
```

---

## 📐 3. Layout Híbrido Responsivo (Pop-up + Painel Lateral Web)

Para permitir que a extensão funcione perfeitamente como **Pop-up de barra de ferramentas**, **Painel Lateral Web (Vivaldi/Brave/Edge)** ou **Aba Completa**:

```css
/* styles.css */
body {
  width: 100%;
  max-width: 100%;
  height: 100vh;
  margin: 0;
  box-sizing: border-box;
  overflow: hidden;
  display: flex;
  flex-direction: column;
}

/* Painéis de conteúdo devem usar flex: 1 com overflow interno */
.panel-content {
  display: none;
  flex: 1;
  flex-direction: column;
  min-height: 0;
  overflow: hidden;
}
.panel-content.active {
  display: flex;
}

/* Listas principais devem preencher até o rodapé */
.main-list {
  flex: 1;
  min-height: 0;
  overflow-y: auto;
}
```

---

## ⏰ 4. Fuso Horário Local Estrito em Métricas e Persistência

Nunca use `toISOString().slice(0, 10)` para chaves de dados do dia a dia, pois isso grava em **UTC (GMT 0)**, causando virada de dia prematura às 21h em fusos como o do Brasil (`UTC-3`).

### Módulo Padrão (`logger.js`):

```javascript
export function getLocalDateString(d = new Date()) {
  const year = d.getFullYear();
  const month = String(d.getMonth() + 1).padStart(2, "0");
  const day = String(d.getDate()).padStart(2, "0");
  return `${year}-${month}-${day}`;
}

export function formatLocalDateTime(dateInput) {
  const d = dateInput instanceof Date ? dateInput : new Date(dateInput);
  return d.toLocaleString("pt-BR", {
    timeZoneName: "short",
    hour12: false
  });
}
```

---

## 🩺 5. Telemetria Local Estruturada para Diagnóstico

Em vez de usar rastreadores externos, registre ações em fila rotativa salva em `chrome.storage.local`:

```javascript
export async function logEvent(level, action, details = {}) {
  const now = new Date();
  const entry = {
    timestamp: now.getTime(),
    localTime: formatLocalDateTime(now),
    dateKey: getLocalDateString(now),
    level, // 'INFO', 'ACTION', 'WARN', 'ERROR'
    action,
    details
  };
  const data = await chrome.storage.local.get(["telemetry_logs"]);
  const logs = data.telemetry_logs || [];
  logs.unshift(entry);
  if (logs.length > 300) logs.pop();
  await chrome.storage.local.set({ telemetry_logs: logs });
}
```

---

## ⚡ 6. Ações Atômicas no Background (Sem Modais Bloqueantes)

- **Nunca use `window.alert()` ou `window.confirm()`** em popups ou side panels; eles são frequentemente bloqueados ou travam o renderizador.
- Dispare mutações de estado via mensagens para o Service Worker (`background.js`).
- Exiba feedback visual imediato na própria UI via componente **Toast auto-dismiss** (4 segundos).

---

## 📦 7. Script Universal Multi-Browser (`build.sh`)

```bash
#!/usr/bin/env bash
set -e

rm -rf dist && mkdir -p dist/chromium dist/firefox

# Chromium: Chrome, Vivaldi, Edge, Brave, Opera, Arc
cp -r icons background.js popup.js popup.html styles.css logger.js manifest.json dist/chromium/
cd dist/chromium && zip -r ../app-chromium.zip . && cd ../..

# Firefox: Firefox, Floorp
cp -r icons background.js popup.js popup.html styles.css logger.js dist/firefox/
cp manifest.firefox.json dist/firefox/manifest.json
cd dist/firefox && zip -r ../app-firefox.xpi . && cd ../..
```
