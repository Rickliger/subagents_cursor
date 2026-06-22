# Team structure (Cursor)

*Exported from `files/dump_files/.cursor/TEAM_STRUCTURE.md`.*

How **subagents**, **rules**, and **skills** fit together for this Go + React dashboard. This file is **reference documentation only** — all behavioral instructions live exclusively in `00-architect.mdc`. **Planning source of truth:** [`doc/00-INDEX.md`](../doc/00-INDEX.md).

---

## Main idea

The main agent (orchestrator) routes each user request to the correct subagent via Task. The user does not need to name a subagent. Before feature work, read **`doc/00-INDEX.md`**. Classify work:

- **Backlog** `doc/backlog/BNNN-*.md` — small fix, one file per item (e.g. B001 price cents, B002 email validation).
- **Spec** `doc/specs/NNNN-*.md` — large feature (e.g. 0001 payment gateway, 0002 report export).
- **Epic** `doc/epics/` — multi-week analysis (E001).

No INDEX, backlog items use subsections **Implementados** / **Propostos** (not a Status column). Specs and epics keep a **Status** column in their table.

Closing a backlog item requires **verifier** (or explicit audit when the user asks *"revisa o que falta"*) before moving the line to **Implementados**. The implementing agent must not update the INDEX alone.

In a fresh chat (no prior context), the architect can audit via prompts: *"Status do projeto"*, *"Revisa o que falta"*, *"Revisa o que já foi"*, or *"Implementa B001"* — see `rules/00-architect.mdc` § Auditoria.

See `rules/00-architect.mdc` for delegation examples.

For large work, multiple subagents of the same type may run in parallel or sequence, each with a clear, non-overlapping scope. When delegating implementation, include `Backlog BNNN` or `Spec 000N` in the Task prompt so subagents read the right acceptance criteria.

Rules apply automatically when a file matching a glob is edited. Subagents run in separate contexts for focused work. Skills are short repeatable workflows in `.cursor/skills/`.

---

## Why this layout

| Benefit | How it works |
|--------|----------------|
| **Context isolation** | Heavy work (DB, research, verification, security review) runs in a dedicated subagent context and does not bloat the main chat. |
| **Parallel work** | Backend + frontend can be delegated simultaneously; or multiple runs of the same subagent handle different slices of one large change. |
| **Right expertise** | Each subagent carries a focused prompt: database vs Go API vs React vs UI vs research vs security. |
| **Consistency** | Glob-scoped rules inject conventions whenever a matching file is opened or edited. |

---

## Subagents (`.cursor/agents/`)

| Subagent | Role | Typical trigger |
|----------|------|-----------------|
| **go-api** | Handlers, services, middleware, routing (Go) | New/refactor API, business logic in Go |
| **database** | SQL, migrations (files only), indexes, repos, query tuning | Migrations created + explained; never executed by the agent — user runs them |
| **react-frontend** | Components, hooks, React Query, features, TS | New/refactor UI logic and data fetching |
| **ui-dashboard** | Layout, KPIs, charts, clarity, density | "Less cluttered", hierarchy, widget patterns |
| **research** | Full codebase dives + web research (cited) + library eval | Route/page analysis, external docs, comparisons; reports in English |
| **testing** | Go + React tests, run suites | New tests, fix failures |
| **verifier** | Skeptical check: implementation + tests vs backlog/spec aceite; gate before INDEX → Implementados | After "done"; confirm behavior; required before closing backlog items |
| **security-reviewer** | Vulnerabilities Go + React | Auth, secrets, sensitive flows, requested audit |

### Grouping

- **Backend:** go-api, database
- **Frontend:** react-frontend, ui-dashboard
- **Cross-cutting:** research, testing, verifier, security-reviewer

---

## Documentation map

| Location | Purpose |
|----------|---------|
| [`doc/00-INDEX.md`](../doc/00-INDEX.md) | Status vitrine — **start here**; backlog in **Implementados** / **Propostos** subsections |
| [`doc/backlog/`](../doc/backlog/) | Small fixes (B001, B002, …) — one file per item; move to Implementados after verifier |
| [`doc/specs/`](../doc/specs/) | Numbered specs with acceptance criteria |
| [`doc/epics/`](../doc/epics/) | Large multi-doc initiatives |
| [`doc/adr/`](../doc/adr/) | Architecture Decision Records |
| [`docs/`](../docs/) | Runbooks (deploy, sandbox, ops) |

---

## Rules (`.cursor/rules/`)

| File | Scope |
|------|--------|
| `00-architect.mdc` | Always — delegation hard rule, project structure, API envelope, boundaries, multi-subagent policy, **doc/00-INDEX** |
| `01-go-api.mdc` | `**/*.go` |
| `02-database.mdc` | Migrations, repos, SQL; migrations: create + explain only |
| `03-react-frontend.mdc` | `frontend/src/**/*.ts(x)` |
| `04-ui-dashboard.mdc` | Components/features TSX, Tailwind config |
| `05-research.mdc` | Research behavior (explicit / when relevant) |
| `06-testing.mdc` | `*_test.go`, frontend `*.test.*` / `*.spec.*` |
| `07-security.mdc` | Auth/middleware/token-related paths, `.env.example` |

---

## Rules vs subagents (reference)

- **Rules** → "How we write in this repo" — conventions injected on file match, and globally for the architect.
- **Subagents** → "Do this chunk of work" — isolated runs chosen and prompted by the main agent.

---

## Skills (`.cursor/skills/`)

Checklist-style flows for repeatable sequences: new endpoint, new React feature, dashboard widget, SQL migration naming. Check related spec in `doc/specs/` and status in `doc/00-INDEX.md` when applicable.

---

## Summary

- One planning index: **`doc/00-INDEX.md`** for humans and all agents.
- 8 subagents covering API, data layer, React, dashboard UX, research, tests, verification, security.
- Database agent writes migration SQL and explains; user applies migrations.
- Research reports are in English; web sources cited when used.
- 8 glob-scoped rules aligned with the stacks above, plus the always-on architect rule.