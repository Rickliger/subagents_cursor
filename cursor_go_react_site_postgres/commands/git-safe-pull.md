---
description: Task → git-pull-reviewer; parent relays report and asks user; pull/stash only after OK in main chat
---

## Routing (mandatory)

The user ran **`/git-safe-pull`**. **Do not** run `git status` / `git fetch` / `git diff` / `git log` yourself or draft the risk report in this chat.

1. Call **Task** with **`subagent_type`: `git-pull-reviewer`** and prompt:

   > Run the full workflow in `.cursor/agents/git-pull-reviewer.md` for this workspace's git repo. Produce Fase 1–2 and the **Handoff (for parent agent)** block (SUMMARY_TAGS, branch status, incoming, outgoing, working tree, conflict analysis, RISK, Options, Parent instructions). **Do not** run `git pull`, `git stash`, `git merge`, `git rebase`, `git reset`, or any destructive command.

2. **After Task returns — you are responsible for the human turn:**
   - Paste or reproduce **at minimum**: `SUMMARY_TAGS`, the **Conflict analysis** table, the **RISK** verdict, and the full **Options** list with the per-option risk lines (user must see the exact PowerShell snippets).
   - Summarize the report **without** dropping the option list.
   - **You ask** the user clearly: which option (A/B/C/D) or a custom path?

3. **Git execution:** Only **after** the user picks an option in **this** chat, you may run the commands for that option yourself, one at a time, following the Parent instructions block:
   - If `git stash pop` produces conflicts, **stop** and report unmerged files (`git diff --name-only --diff-filter=U`).
   - If `git pull --ff-only` fails because history diverged, **stop** and ask whether to use `--rebase` or `--no-ff`. Do not pick silently.
   - After success, run `git status -sb` and `git log -1 --oneline`, then report the new HEAD.
   - If lockfile changed, **suggest** `npm install` — do not run without OK.
   - Never touch `.env*` files.

**Do not** collapse the subagent result into a vague "looks safe, pulling…" line without showing **SUMMARY_TAGS**, **RISK**, and the **Options** list.

**If Task is unavailable:** say so and stop (unless the user explicitly asks for a fallback).
