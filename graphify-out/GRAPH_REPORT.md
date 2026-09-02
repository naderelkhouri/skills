# Graph Report - skills  (2026-09-02)

## Corpus Check
- 3 files · ~2,234 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 32 nodes · 29 edges · 5 communities
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `4654ff2b`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- [[_COMMUNITY_Community 0|Community 0]]
- [[_COMMUNITY_Community 1|Community 1]]
- [[_COMMUNITY_Community 2|Community 2]]
- [[_COMMUNITY_Community 3|Community 3]]
- [[_COMMUNITY_Community 4|Community 4]]

## God Nodes (most connected - your core abstractions)
1. `Skill: SEO & AEO Technical Auditor (Search + Answer Engine Optimization)` - 7 edges
2. `Antigravity & Agent Skills Catalog` - 5 edges
3. `Skill: Cloudflare Pages & Functions (Deploy, Troubleshooting & Cache Control)` - 4 edges
4. `3. 🛠️ Fluxo Padrão de Deploy & Validação` - 4 edges
5. `4. 🔬 Protocolo de Auditoria e Comandos de Diagnóstico` - 4 edges
6. `🚀 Como Utilizar` - 3 edges
7. `1. 🚨 Armadilhas de Deploy & Resolução de Problemas (Troubleshooting)` - 3 edges
8. `1. 🧠 Filosofia Central & Equação de Valor` - 2 edges
9. `2. 🏛️ As 5 Camadas Arquiteturais & 20 Configurações Essenciais` - 2 edges
10. `3. 📊 Matriz de Pontuação: "SEO & AEO Technical Score" (0–100)` - 2 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Import Cycles
- None detected.

## Communities (5 total, 0 thin omitted)

### Community 0 - "Community 0"
Cohesion: 0.20
Nodes (9): 1. 🧠 Filosofia Central & Equação de Valor, 2. 🏛️ As 5 Camadas Arquiteturais & 20 Configurações Essenciais, 3. 📊 Matriz de Pontuação: "SEO & AEO Technical Score" (0–100), 5. 📋 Template do Relatório Canônico de Auditoria, 6. 🚀 Integração e Produto AdmX.Tech, A Equação de Ranqueamento & Citação:, Faixas de Classificação:, Skill: SEO & AEO Technical Auditor (Search + Answer Engine Optimization) (+1 more)

### Community 1 - "Community 1"
Cohesion: 0.25
Nodes (7): 1. Instalação Global no Antigravity (`~/.gemini/config/skills`), 2. Uso Específico por Projeto (`.agents/skills/`), Antigravity & Agent Skills Catalog, 👤 Autor, 📚 Catálogo de Skills, 🚀 Como Utilizar, 🏛️ Estrutura de uma Skill

### Community 2 - "Community 2"
Cohesion: 0.33
Nodes (5): 1. 🚨 Armadilhas de Deploy & Resolução de Problemas (Troubleshooting), 2. ⚡ Paridade de Backend: Express vs Cloudflare Functions, A. Prevenção de Tela Branca por Fallback SPA Incorreto (`200.html`), B. Gestão de Caching de Borda no Edge CDN (`public/_headers`), Skill: Cloudflare Pages & Functions (Deploy, Troubleshooting & Cache Control)

### Community 3 - "Community 3"
Cohesion: 0.50
Nodes (4): 3. 🛠️ Fluxo Padrão de Deploy & Validação, Passo 1: Limpeza e Compilação Limpa, Passo 2: Implantação no Cloudflare Pages com Wrangler, Passo 3: Auditoria Empírica dos Endpoints em Produção

### Community 4 - "Community 4"
Cohesion: 0.50
Nodes (4): 4. 🔬 Protocolo de Auditoria e Comandos de Diagnóstico, A. Validação de Crawlability & Headers (via CLI), B. Validação On-Page & Schema.org, C. Checklist de Verificação para AEO (Answer Engine Optimization)

## Knowledge Gaps
- **19 isolated node(s):** `📚 Catálogo de Skills`, `1. Instalação Global no Antigravity (`~/.gemini/config/skills`)`, `2. Uso Específico por Projeto (`.agents/skills/`)`, `🏛️ Estrutura de uma Skill`, `👤 Autor` (+14 more)
  These have ≤1 connection - possible missing edges or undocumented components.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Skill: SEO & AEO Technical Auditor (Search + Answer Engine Optimization)` connect `Community 0` to `Community 4`?**
  _High betweenness centrality (0.148) - this node is a cross-community bridge._
- **Why does `4. 🔬 Protocolo de Auditoria e Comandos de Diagnóstico` connect `Community 4` to `Community 0`?**
  _High betweenness centrality (0.071) - this node is a cross-community bridge._
- **Why does `Skill: Cloudflare Pages & Functions (Deploy, Troubleshooting & Cache Control)` connect `Community 2` to `Community 3`?**
  _High betweenness centrality (0.058) - this node is a cross-community bridge._
- **What connects `📚 Catálogo de Skills`, `1. Instalação Global no Antigravity (`~/.gemini/config/skills`)`, `2. Uso Específico por Projeto (`.agents/skills/`)` to the rest of the system?**
  _19 weakly-connected nodes found - possible documentation gaps or missing edges._