# Team structure (Cursor)

How **subagents**, **rules**, and **skills** fit together for this **Next.js App Router** **public marketing website** (**Tailwind CSS v4**, `app/` tree). The primary surface is **customer-facing** pages (SEO, performance, conversion). An **admin panel** may appear later; until then, avoid premature internal-dashboard patterns on the public app. This file is **reference documentation only** — behavioral routing rules live in `rules/00-architect.mdc`.

---

## Main idea

The main agent (orchestrator) routes each user request to the correct subagent via Task. The user does not need to name a subagent. For large work, multiple subagents of the same type may run in parallel or sequence, each with a clear, non-overlapping scope.

Rules apply automatically when a file matching a glob is edited. Subagents run in separate contexts for focused work. Skills are short repeatable workflows in `.cursor/skills/`.

---

## Why this layout

| Benefit | How it works |
|--------|----------------|
| **Context isolation** | Heavy work (research, verification, security review) runs in a dedicated subagent context and does not bloat the main chat. |
| **Parallel work** | Multiple UI passes or tests can be delegated in parallel when scopes do not overlap. |
| **Right expertise** | Each subagent carries a focused prompt: Next/React vs marketing UI vs research vs security. |
| **Consistency** | Glob-scoped rules inject conventions whenever a matching file is opened or edited. |

---

## Subagents (`.cursor/agents/`)

| Subagent | Role | Typical trigger |
|----------|------|-----------------|
| **go-api** | Go handlers, services, middleware *(dormant until `*.go` exists)* | New/refactor Go HTTP |
| **database** | SQL, migrations (files only), indexes *(dormant until migrations/repos exist)* | Schema changes — agent explains; user runs migrations |
| **react-frontend** | `app/` pages/layouts, components, hooks, **`app/api/**` Route Handlers**, TS/JS | New routes, UI logic, server/client boundaries |
| **ui-dashboard** | Public site **layout**: heroes, sections, hierarchy, a11y, spacing *(future admin may reuse dashboard-style patterns when scoped)* | "Less cluttered", visual polish, section structure |
| **research** | Codebase dives + web research (cited) + library eval | Route/page analysis, external docs; reports in English |
| **testing** | Tests for stack present in repo | New tests, fix failures |
| **verifier** | Skeptical check: implementation + tests | After "done"; confirm behavior |
| **security-reviewer** | Vulnerabilities (Next + optional Go) | Auth, secrets, Route Handlers, requested audit |
| **git-commiter** | Revisa git → propõe **COMMIT_SUBJECT** (só tag) + **COMMIT_BODY** (PT) → handoff para o orchestrator (confirmação na main chat); não commita sozinho | Task → relay na main → git só após OK |

### Grouping

- **Backend (optional):** go-api, database — when Go/SQL land in the monorepo
- **Frontend:** react-frontend, ui-dashboard
- **Design system:** Canonical spec lives in `doc/design-doc/`; CSS tokens and `.type-*` typography utilities live in `app/globals.css`. The `react-frontend` and `ui-dashboard` subagents must consult both for any visual change.
- **Cross-cutting:** research, testing, verifier, security-reviewer, git-commiter

---

## Rules (`.cursor/rules/`)

| File | Scope |
|------|--------|
| `00-architect.mdc` | Always — delegation, project structure, Next-focused boundaries |
| `01-go-api.mdc` | `**/*.go` *(inactive until Go files exist)* |
| `02-database.mdc` | SQL/migrations when present |
| `03-react-frontend.mdc` | `app/**/*.tsx`, `app/**/*.jsx`, `app/**/*.js` |
| `04-ui-dashboard.mdc` | Marketing UI: `app/**/*.tsx`, `app/**/*.jsx`, `app/components/**/*`, `app/globals.css`, `postcss.config.mjs` |
| `05-research.mdc` | Research behavior |
| `06-testing.mdc` | Go tests when present; `app/**/*.test.*`, `app/**/*.spec.*`, e2e globs |
| `07-security.mdc` | Auth/sensitive paths, **`app/api/**`**, `.env.example` |
| `08-nextjs.mdc` | `app/**/*`, `next.config.mjs` — Next docs, App Router, Route Handlers |

---

## Rules vs subagents (reference)

- **Rules** → "How we write in this repo" — conventions injected on file match, and globally for the architect.
- **Subagents** → "Do this chunk of work" — isolated runs chosen and prompted by the main agent.

---

## Slash commands (`.cursor/commands/`)

| Command | Role |
|---------|------|
| **`/lets-commit`** | Task → **git-commiter**; relay **COMMIT_SUBJECT** / **COMMIT_BODY**; you confirm in main chat. |
| **`/commit`** | After confirmation in the **same** chat: stage, **`git commit`**, **`git push`** (optional tag if agreed). |

## Skills (`.cursor/skills/`)

Checklist-style flows: **nextjs-new-route**, dashboard widget (if used), SQL migration (when DB exists), Go endpoint (when API exists).

---

## Summary

- Subagents cover Next UI, **public-site** marketing UX, optional Go/DB, research, tests, verification, security; **admin** is out of scope until explicitly planned.
- **Tailwind v4** via `app/globals.css` + `@tailwindcss/postcss` — no root `tailwind.config.*`.
- Research reports are in English; web sources cited when used.
- Glob-scoped rules match **`app/`** and Next config, plus dormant Go/SQL rules for a future monorepo.
