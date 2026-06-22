# Team structure (Cursor)

How **subagents**, **rules**, and **skills** fit together for this **Next.js App Router** **public marketing website** (**Tailwind CSS v4**, `app/` tree) with optional **Go API + PostgreSQL**. The primary surface is **customer-facing** pages (SEO, performance, conversion). An **admin panel** may appear later; until then, avoid premature internal-dashboard patterns on the public app. This file is **reference documentation only** — behavioral routing rules live in `rules/00-architect.mdc`.

**Planning source of truth:** `doc/00-INDEX.md` (in the consumer repo — create when adopting this pack).

---

## Main idea

The main agent (orchestrator) routes each user request to the correct subagent via Task. The user does not need to name a subagent. For large work, multiple subagents of the same type may run in parallel or sequence, each with a clear, non-overlapping scope.

Before feature work, read **`doc/00-INDEX.md`**. Classify work:

| Tipo | Onde |
|------|------|
| Fix pequeno | `doc/backlog/BNNN-*.md` — INDEX: **Implementados** / **Propostos** |
| Feature | `doc/specs/NNNN-*.md` — coluna Status |
| Epic | `doc/epics/` |

**Fechar item:** `[x]` + **verifier** (ou revisão doc+código) + INDEX. Quem implementou não fecha sozinho.

**Chat zerado:** *"status do projeto"* | *"revisa o que falta"* (auditoria, sem codar) | *"revisa o que já foi"* | *"implementa B001"*.

See `rules/00-architect.mdc` for delegation examples and the distinction between visual design (`doc/design-doc/`) and product planning (`doc/specs/`, `doc/backlog/`).

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
| **research** | Codebase dives + web research (cited) + library eval | Route/page analysis, `doc/00-INDEX`, `doc/specs/`, external docs; reports in English |
| **testing** | Tests for stack present in repo | New tests, fix failures |
| **verifier** | Gate before INDEX closure: skeptical check vs aceite + tests | After implementation; **required** before marking implementado / moving backlog to **Implementados** |
| **security-reviewer** | Vulnerabilities (Next + optional Go) | Auth, secrets, Route Handlers, requested audit |
| **git-commiter** | Revisa git → propõe **COMMIT_SUBJECT** (só tag) + **COMMIT_BODY** (PT) → handoff para o orchestrator (confirmação na main chat); não commita sozinho | Task → relay na main → git só após OK |

### Grouping

- **Backend (optional):** go-api, database — when Go/SQL land in the monorepo
- **Frontend:** react-frontend, ui-dashboard
- **Design system (visual):** Canonical spec lives in `doc/design-doc/`; CSS tokens and `.type-*` typography utilities live in `app/globals.css`. The `react-frontend` and `ui-dashboard` subagents must consult both for any visual change.
- **Product planning:** Status vitrine in `doc/00-INDEX.md`; backlog **Implementados** / **Propostos**; acceptance criteria in `doc/specs/` and `doc/backlog/`. **verifier** gates INDEX closure. All agents read these when cited in the task prompt (`Backlog B001`, `Spec 0002`).
- **Cross-cutting:** research, testing, verifier, security-reviewer, git-commiter

---

## Documentation map

| Location | Purpose |
|----------|---------|
| `doc/00-INDEX.md` | Planning vitrine — **start here** for specs, backlog, epics |
| `doc/design-doc/` | **Visual** design system — colors, typography, components, motion, a11y |
| `doc/specs/` | Numbered feature specs with acceptance criteria (`0001`, `0002`, …) |
| `doc/backlog/` | Small fixes — one file per item (`B001`, `B002`, …); INDEX subsections **Implementados** / **Propostos** |
| `doc/epics/` | Large multi-doc initiatives (`E001`, …) |
| `doc/adr/` | Architecture decision records |
| `docs/` | Runbooks (deploy, CDN, operation) — optional |

**Do not confuse:** `doc/design-doc/` answers *how it looks*; `doc/specs/` and `doc/backlog/` answer *what to build* and *when it is done*.

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

Checklist-style flows: **nextjs-new-route**, **react-new-feature**, dashboard widget (if used), SQL migration (when DB exists), Go endpoint (when API exists). Check related spec in `doc/specs/` or backlog item in `doc/backlog/` when applicable.

---

## Summary

- Subagents cover Next UI, **public-site** marketing UX, optional Go/DB, research, tests, verification, security; **admin** is out of scope until explicitly planned.
- **Planning:** one index — **`doc/00-INDEX.md`** — for humans and all agents; backlog **Implementados** / **Propostos**; **verifier** before closing items; visual design stays in **`doc/design-doc/`**.
- **Tailwind v4** via `app/globals.css` + `@tailwindcss/postcss` — no root `tailwind.config.*`.
- Research reports are in English; web sources cited when used.
- Glob-scoped rules match **`app/`** and Next config, plus dormant Go/SQL rules for a future monorepo.
