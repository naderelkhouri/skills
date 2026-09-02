---
name: cloudflare-pages-deploy
description: Diretrizes técnicas, troubleshooting, controle de cache CDN e integração serverless (Functions + D1) para aplicações Cloudflare Pages.
---

# Skill: Cloudflare Pages & Functions (Deploy, Troubleshooting & Cache Control)

Esta skill consolida as melhores práticas, diagnósticos de erros de produção e estratégias de implantação para aplicações React/Vite com backend Serverless no **Cloudflare Pages** e **Cloudflare Functions**.

---

## 1. 🚨 Armadilhas de Deploy & Resolução de Problemas (Troubleshooting)

### A. Prevenção de Tela Branca por Fallback SPA Incorreto (`200.html`)
* **Sintoma**: O site em produção apresenta tela em branco (*blank screen*) e os logs do navegador apontam `SyntaxError: Unexpected token '<'` ou erro ao carregar `/src/main.tsx`.
* **Causa Raiz**: O Cloudflare Pages busca um arquivo `200.html` para roteamento SPA de fallback. Se a pasta `public/` contiver um `200.html` legado apontando para código fonte não compilado (`/src/main.tsx`), o servidor entregará HTML quando o navegador esperar JavaScript.
* **Solução**:
  1. Deletar qualquer arquivo `public/200.html`.
  2. Utilizar a regra nativa de fallback SPA no arquivo `public/_redirects`:
     ```text
     /*  /index.html  200
     ```

---

### B. Gestão de Caching de Borda no Edge CDN (`public/_headers`)
* **Sintoma**: Após publicar uma nova build, o domínio personalizado (`https://admx.tech`) continua carregando hashes velhos de JavaScript (`index-old.js`), resultando em erros 404 e comportamento inconsistente (`cf-cache-status: HIT`).
* **Causa Raiz**: O Edge CDN do Cloudflare pode reter o `index.html` em cache de borda. Como a build do Vite gera novos hashes para os arquivos em `/assets/`, o HTML antigo requisita scripts que foram excluídos.
* **Solução**: Configurar explicitamente as diretivas de cache no arquivo `public/_headers`:
  ```text
  /index.html
    Cache-Control: no-cache, no-store, must-revalidate

  /assets/*
    Cache-Control: public, max-age=31536000, immutable

  /*
    X-Frame-Options: SAMEORIGIN
    X-Content-Type-Options: nosniff
    Referrer-Policy: strict-origin-when-cross-origin
    Permissions-Policy: camera=(), microphone=(), geolocation=()
    Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
  ```

---

## 2. ⚡ Paridade de Backend: Express vs Cloudflare Functions

Para garantir que a aplicação funcione tanto no ambiente de desenvolvimento local (Node.js/Express) quanto no servidor de produção (Cloudflare Serverless), a arquitetura deve manter paridade estrita nos contratos de API:

| Rota API | Servidor Local (`server.ts`) | Cloudflare Pages (`functions/api/*.js`) | Função |
| :--- | :--- | :--- | :--- |
| `/api/diagnostico` | Handler Express com `@google/genai` | `functions/api/diagnostico.js` | Diagnóstico estratégico 80/20 via Gemini API com fallback em mockData estruturado. |
| `/api/leads` | Handler Express (memória local) | `functions/api/leads.js` | Persistência de leads no Cloudflare D1 (`env.DB`) e envio de e-mails via MailChannels. |
| `/api/chat-ia` | Handler Express com `@google/genai` | `functions/api/chat-ia.js` | WhatsApp Agent de IA com sanitização de prompt, rate-limiting e respostas dinâmicas. |

---

## 3. 🛠️ Fluxo Padrão de Deploy & Validação

### Passo 1: Limpeza e Compilação Limpa
```bash
npm run clean && npm run build
```

### Passo 2: Implantação no Cloudflare Pages com Wrangler
```bash
npx wrangler pages deploy dist --project-name=admx-tech
```

### Passo 3: Auditoria Empírica dos Endpoints em Produção
```bash
# 1. Teste de Cabeçalhos e Asset principal
curl -I https://admx.tech

# 2. Teste da API de Diagnóstico
curl -s -X POST https://admx.tech/api/diagnostico \
  -H "Content-Type: application/json" \
  -d '{"empresaName": "Teste Skill", "niche": "Tecnologia", "mainPain": "Processos manuais"}'

# 3. Teste da API de Captura de Leads (D1)
curl -s -X POST https://admx.tech/api/leads \
  -H "Content-Type: application/json" \
  -d '{"companyName": "Teste Skill Lead", "niche": "Tech", "mainBottleneck": "Automação", "source": "skill_test"}'

# 4. Listagem de Leads no Cloudflare D1
curl -s https://admx.tech/api/leads
```
