# Team structure (Cursor)

How **subagents**, **rules**, and **skills** fit together for this **Go + Angular + Postgres** dashboard. This file is **reference documentation only** — all behavioral instructions live exclusively in `00-architect.mdc`.

---

## Main idea

The main agent (orchestrator) routes each user request to the correct subagent via Task. The user does not need to name a subagent. For large work, multiple subagents of the same type may run in parallel or sequence, each with a clear, non-overlapping scope.

Rules apply automatically when a file matching a glob is edited. Subagents run in separate contexts for focused work. Skills are short repeatable workflows in `.cursor/skills/`.

---

## Why this layout

| Benefit | How it works |
|--------|----------------|
| **Context isolation** | Heavy work (DB, research, verification, security review) runs in a dedicated subagent context and does not bloat the main chat. |
| **Parallel work** | Backend + frontend can be delegated simultaneously; or multiple runs of the same subagent handle different slices of one large change. |
| **Right expertise** | Each subagent carries a focused prompt: database vs Go API vs Angular vs UI vs research vs security. |
| **Consistency** | Glob-scoped rules inject conventions whenever a matching file is opened or edited. |

---

## Subagents (`.cursor/agents/`)

| Subagent | Role | Typical trigger |
|----------|------|-----------------|
| **go-api** | Handlers, services, middleware, routing (Go) | New/refactor API, business logic in Go |
| **database** | SQL, migrations (files only), indexes, repos, query tuning | Migrations created + explained; never executed by the agent — user runs them |
| **angular-frontend** | Components, services, routes, HttpClient, RxJS, templates | New/refactor UI logic and API integration |
| **ui-dashboard** | Layout, KPIs, charts, clarity, density | "Less cluttered", hierarchy, widget patterns |
| **research** | Full codebase dives + web research (cited) + library eval | Route/page analysis, external docs, comparisons; reports in English |
| **testing** | Go + Angular tests, run suites | New tests, fix failures |
| **verifier** | Skeptical check: implementation + tests | After "done"; confirm behavior |
| **security-reviewer** | Vulnerabilities Go + Angular | Auth, secrets, sensitive flows, requested audit |

### Grouping

- **Backend:** go-api, database
- **Frontend:** angular-frontend, ui-dashboard
- **Cross-cutting:** research, testing, verifier, security-reviewer

---

## Rules (`.cursor/rules/`)

| File | Scope |
|------|--------|
| `00-architect.mdc` | Always — delegation hard rule, project structure, API envelope, boundaries, multi-subagent policy |
| `01-go-api.mdc` | `**/*.go` |
| `02-database.mdc` | Migrations, repos, SQL; migrations: create + explain only |
| `03-angular-frontend.mdc` | `frontend/src/app/**/*.ts`, `**/*.html` under app |
| `04-ui-dashboard.mdc` | Component templates/styles, Tailwind config |
| `05-research.mdc` | Research behavior (explicit / when relevant) |
| `06-testing.mdc` | `*_test.go`, `frontend/src/**/*.spec.ts` |
| `07-security.mdc` | Auth/middleware/token-related paths, `.env.example` |

---

## Rules vs subagents (reference)

- **Rules** → "How we write in this repo" — conventions injected on file match, and globally for the architect.
- **Subagents** → "Do this chunk of work" — isolated runs chosen and prompted by the main agent.

---

## Skills (`.cursor/skills/`)

Checklist-style flows for repeatable sequences: new endpoint, new Angular feature, dashboard widget, SQL migration naming.

---

## Summary

- 8 subagents covering API, data layer, Angular, dashboard UX, research, tests, verification, security.
- Database agent writes migration SQL and explains; user applies migrations.
- Research reports are in English; web sources cited when used.
- 8 glob-scoped rules aligned with the stacks above, plus the always-on architect rule.
