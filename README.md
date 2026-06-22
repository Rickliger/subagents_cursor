# subagents_cursor

Repositório de **template packs** para Cursor (subagents, rules, skills) e **starter de documentação** de planejamento (specs, backlog, épicos, ADRs).

---

## Template packs (`.cursor/`)

Três packs prontos para copiar para a pasta `.cursor/` do projeto alvo:

| Pack | Stack | Uso típico |
|------|-------|------------|
| [`cursor_go_react_postgres/`](cursor_go_react_postgres/) | Go + React + Postgres | Dashboard interno com API REST |
| [`cursor_go_react_site_postgres/`](cursor_go_react_site_postgres/) | Go + Next.js (App Router) + Postgres | Site público / marketing + API |
| [`cursor_go_angular_postgres/`](cursor_go_angular_postgres/) | Go + Angular + Postgres | Dashboard interno com Angular |

Cada pack inclui:

- `agents/` — subagents especializados (go-api, database, frontend, testing, etc.)
- `rules/` — regras globais e por camada (`00-architect.mdc`, `01-go-api.mdc`, …)
- `skills/` — fluxos repetíveis (novo endpoint, migration, nova feature)
- `TEAM_STRUCTURE.md` — referência de papéis e delegação

### Instalação do pack

Copie o conteúdo do pack escolhido para `.cursor/` na raiz do projeto:

```bash
# Exemplo: dashboard Go + React
cp -r cursor_go_react_postgres/* /caminho/do/projeto/.cursor/
```

Ajuste paths e nomes no `TEAM_STRUCTURE.md` se o layout do projeto divergir do padrão do pack.

---

## Starter de planejamento (`doc/`)

A pasta [`doc-starter/`](doc-starter/) contém um modelo genérico de **spec/backlog system** — independente do stack.

### Instalação

Copie para a raiz do projeto como `doc/`:

```bash
cp -r doc-starter/ /caminho/do/projeto/doc/
```

Abra `doc/00-INDEX.md` e personalize o título. Remova ou substitua `doc/specs/0001-example-feature.md` ao registrar a primeira spec real.

### Como funciona

| ID | Onde | Quando usar |
|----|------|-------------|
| **B001, B002, …** | `doc/backlog/BNNN-*.md` | Bug ou fix pequeno — **um arquivo por item** |
| **0001, 0002, …** | `doc/specs/NNNN-*.md` | Feature ou integração grande |
| **E001, E002, …** | `doc/epics/<nome>/` | Épico longo (migration, refactor multi-doc) |

O [`00-INDEX.md`](doc-starter/00-INDEX.md) é a vitrine: specs/épicos com coluna Status; backlog em subseções **Implementados** / **Propostos** (sem coluna Status); o que está em andamento; links para cada item. Agents em `.cursor/` (especialmente o arquiteto) consultam o INDEX antes de delegar trabalho. Ao fechar item, preferir **verifier** antes de marcar implementado — quem implementou não fecha sozinho.

**Runbooks** (deploy, operação, sandbox) ficam em `docs/` na raiz — separado de `doc/`.

---

## Estrutura deste repositório

```
subagents_cursor/
├── cursor_go_react_postgres/      # pack dashboard React
├── cursor_go_react_site_postgres/ # pack site Next.js
├── cursor_go_angular_postgres/    # pack dashboard Angular
├── doc-starter/                   # copiar → doc/ no projeto
└── README.md
```
