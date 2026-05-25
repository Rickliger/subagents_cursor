---
description: Stage, commit with agreed COMMIT_SUBJECT/BODY, push — after /lets-commit + OK in same chat
---

## When to use

User ran **`/commit`**. Intended flow: **`/lets-commit`** → user confirms **COMMIT_SUBJECT** + **COMMIT_BODY** → **`/commit`** in the **same** chat so the agreed strings are still in context.

## Mandatory checks

1. **Conversation context:** You need a clear **COMMIT_SUBJECT** (first line only, usually `vX.Y.Z`) and **COMMIT_BODY** (Portuguese bullets) that the user **already approved** in this thread.  
   - If missing or fuzzy, **stop** and ask them to paste both, or run **`/lets-commit`** first.

2. **Do not** write a new commit message without that approval.

## Steps (repo root)

`cd` to the git root (e.g. `C:/Github/Painel-Backoffice`).

1. **`git status -sb`** — show what will go in; warn if junk or secrets (`.env`, keys, build noise) look staged.

2. **Stage** what belongs to this commit — typically **`git add -A`** unless the user limited paths. Honor `.gitignore`.

3. **`git commit`** with exactly:
   - **Subject:** `COMMIT_SUBJECT`
   - **Body:** `COMMIT_BODY`  
   Use reliable quoting for the shell (e.g. several `-m` args for paragraphs, or `git commit -F` with a short temp file if newlines are awkward on Windows).

4. **`git push`** to the current branch’s upstream (`git push` when upstream exists, else `git push -u origin <branch>`).  
   - On failure: report output; **no** `--force` unless the user explicitly asks.

## Optional tag (only if user already said yes in this chat)

If they confirmed an **annotated tag** with the same name as **COMMIT_SUBJECT** (e.g. `v2.2.0`), create it **after** commit and **before** push **only** when that tag does not already exist:

`git tag -a <COMMIT_SUBJECT> -m "Release <COMMIT_SUBJECT>"`

If they did **not** mention tagging, **skip** tagging.

## Safety

- No **`git push --force`** without explicit user request.  
- If secrets or wrong files are staged, stop and warn before committing.
