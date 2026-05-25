---
description: >-
  Open a new tab in Cursor’s Simple Browser (cursor-ide-browser MCP); optional side panel.
---

## When to use

The user ran **`/new-browser`** (or asked to open a new browser tab in the IDE).

## Do this

1. **Tool:** **`cursor-ide-browser`** → **`browser_tabs`**.
2. **Arguments:** `{ "action": "new" }`.

**Side by side:** If the user asked for the browser **beside** the editor, **side panel**, or **side by side**, use:

```json
{ "action": "new", "position": "side" }
```

3. **Reply:** Confirm the new tab (title / URL from tool metadata if returned). If the user also gave a **URL**, call **`browser_navigate`** with that `url` (same tab; omit `newTab` unless they want another tab).

## Do not

- Assume an external Chrome/Edge window — this command is for **Cursor’s integrated browser** via MCP.
- Skip reading MCP tool schemas if you are unsure of parameters; **`browser_tabs`** is documented under `mcps/cursor-ide-browser/tools/browser_tabs.json`.

## Reference (IDE workflow)

After **`action: "new"`**, optional navigation uses **`browser_navigate`** (`url` required; `newTab: true` only if they explicitly want a second tab from an existing one).
