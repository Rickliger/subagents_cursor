---
description: Task → git-commiter; parent relays proposal and asks user; git only after OK in main chat
---

## Routing (mandatory)

The user ran **`/lets-commit`**. **Do not** run `git status` / `git diff` / `git log` yourself or draft the commit review in this chat.

1. Call **Task** with **`subagent_type`: `git-commiter`** and prompt:

   > Run the full workflow in `.cursor/agents/git-commiter.md` for this workspace’s git repo (working tree: staged + unstaged + likely untracked). Produce Phase 1–2 and the **Handoff (for parent agent)** block. **Do not** run `git commit`, `git tag`, or `git push`.

2. **After Task returns — you are responsible for the human turn:**
   - Paste or reproduce **at minimum** the **COMMIT_SUBJECT** and full **COMMIT_BODY** from the handoff (user must see exact strings).
   - Summarize the review **without** dropping the proposal.
   - **You ask** the user clearly: OK with this tag and body, or what to change?

3. **Git execution:** Only **after** the user confirms in **this** chat, you **may** run `git add` / `git commit` / `git tag` / `git push` yourself — **or** tell them to run **`/commit`** next (same chat) to stage, commit, and push using `.cursor/commands/commit.md`.

**Do not** collapse the subagent result into a vague “Phase 3 confirm…” line without showing **COMMIT_SUBJECT** + **COMMIT_BODY**.

**If Task is unavailable:** say so and stop (unless the user explicitly asks for a fallback).
