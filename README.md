# Antigravity & Agent Skills Catalog

Coleção canônica de **Skills especializadas, diretrizes de engenharia e runbooks técnicos** para o **Google Antigravity (AGY)**, agentes autônomos de desenvolvimento e ecossistemas de IA.

---

## 📚 Catálogo de Skills

| Skill | Descrição | Escopo / Aplicação |
| :--- | :--- | :--- |
| [`seo-aeo-audit`](./seo-aeo-audit/SKILL.md) | Framework de auditoria técnica de SEO e AEO (*Search Engine + Answer Engine Optimization*) em 5 camadas e 20 configurações essenciais. Avalia rastreabilidade, performance técnica, on-page, E-E-A-T e citabilidade por LLMs (ChatGPT, Gemini, Perplexity), calculando o **SEO Technical Score (0–100)** e plano de remediação. | SEO Técnico, AEO, Auditoria de Websites, AdmX.Tech |
| [`cloudflare-pages`](./cloudflare-pages/SKILL.md) | Diretrizes técnicas, diagnósticos de tela branca (*SPA fallback* `_redirects`), gestão de cache de borda (*Edge CDN* `_headers`) e paridade de backend Express vs Cloudflare Functions + D1. | Cloudflare Pages, Edge Computing, Deploy Serverless |

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
