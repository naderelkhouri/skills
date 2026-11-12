# Antigravity & Agent Skills Catalog

Coleção canônica de **Skills especializadas, diretrizes de engenharia e runbooks técnicos** para o **Google Antigravity (AGY)**, agentes autônomos de desenvolvimento e ecossistemas de IA.

---

## 📚 Catálogo de Skills

### SEO e Deploy

| Skill | Descrição |
| :--- | :--- |
| [`seo-aeo-audit`](./seo-aeo-audit/SKILL.md) | Auditoria técnica de SEO e AEO em 5 camadas, com SEO Technical Score (0–100). |
| [`cloudflare-pages`](./cloudflare-pages/SKILL.md) | Cloudflare Pages: SPA fallback, cache de borda, Functions + D1. |

### Planejamento e entrega

| Skill | Descrição |
| :--- | :--- |
| [`grill-me`](./grill-me/SKILL.md) | Entrevista dura para afiar plano ou design. |
| [`to-tickets`](./to-tickets/SKILL.md) | Plano/spec/conversa → tickets com dependências. |
| [`handoff`](./handoff/SKILL.md) | Compacta a conversa para outro agente continuar. |
| [`improve-codebase-architecture`](./improve-codebase-architecture/SKILL.md) | Acha oportunidades de deepening na codebase e gera relatório HTML. |

### UI, telas e design system

| Skill | Descrição |
| :--- | :--- |
| [`plan-screen`](./plan-screen/SKILL.md) | Wireframes ASCII tipados de telas/fluxos, depois PNGs. |
| [`img2html`](./img2html/SKILL.md) | Mock (imagem) → HTML estático em uma passada, com 2 aprovações. |
| [`video2html`](./video2html/SKILL.md) | Vídeo de UI animada → página HTML viva (GSAP, shaders, Three.js). |
| [`extract-ds`](./extract-ds/SKILL.md) | Site pronto → Design System funcional com aprovação. |

### Mídia e docs

| Skill | Descrição |
| :--- | :--- |
| [`mediabunny`](./mediabunny/SKILL.md) | Áudio/vídeo com a lib Mediabunny. |
| [`context7-mcp`](./context7-mcp/SKILL.md) | Docs atuais de libs via Context7. |

> Planejamento, UI e mídia foram importadas de [rtadewald/skills](https://github.com/rtadewald/skills).

---

## 🚀 Como Utilizar

### 1. Instalação Global no Antigravity (`~/.gemini/config/skills`)

Para disponibilizar todas as skills em qualquer workspace do Antigravity na sua máquina:

```bash
# Clone diretamente para o diretório de configurações globais
git clone https://github.com/naderelkhouri/skills.git ~/.gemini/config/skills
```

Ou crie links simbólicos a partir deste repositório:

```bash
ln -s /home/nader/Documents/skills/seo-aeo-audit ~/.gemini/config/skills/seo-aeo-audit
ln -s /home/nader/Documents/skills/cloudflare-pages ~/.gemini/config/skills/cloudflare-pages
```

### 2. Uso Específico por Projeto (`.agents/skills/`)

Para versionar uma skill dentro de um repositório específico:

```bash
mkdir -p .agents/skills
cp -r /caminho/para/skills/seo-aeo-audit .agents/skills/
```

---

## 🏛️ Estrutura de uma Skill

Cada skill segue o padrão de *Progressive Disclosure* do Antigravity:

```text
skills/
├── <nome-da-skill>/
│   ├── SKILL.md          # Instruções canônicas com YAML frontmatter (name, description)
│   ├── scripts/          # (Opcional) Scripts e ferramentas auxiliares
│   ├── references/       # (Opcional) Documentação e referências técnicas
│   └── examples/         # (Opcional) Exemplos práticos de implementação
```

---

## 👤 Autor

Desenvolvido e mantido por **[Nader El Khouri](https://github.com/naderelkhouri)**.
