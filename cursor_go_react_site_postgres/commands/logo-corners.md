---
description: Apply Meu Consig logo-format border-radius to the selected UI — pick xs/sm/md/lg/xl from tokens
---

## When to use

The user ran **`/logo-corners`** (or equivalent) with a **selected DOM element** or pasted **DOM path / HTML snippet / component file path**. Goal: use the **logo-format radius** (three corners share **R**, bottom-right tighter ≈ **R÷4**) documented in `doc/design-doc/04-spacing-grid-layout.md` §8.1.

## Rules

1. **Tokens only** — no new arbitrary tuples. Use `--radius-logo-xs` … `--radius-logo-xxl` from `app/globals.css` via:
   - **`rounded-logo-xs` | `rounded-logo-sm` | `rounded-logo-md` | `rounded-logo-lg` | `rounded-logo-xl` | `rounded-logo-xxl`**, or
   - if utilities cannot be applied, one explicit arbitrary class such as **`rounded-[var(--radius-logo-md)]`** (substitute: `xs` | `sm` | `md` | `lg` | `xl` | `xxl`).

2. **Pick size from element scale** (approximate):
   - **xs** — compact chips, narrow tiles (&lt; ~120 px wide), dense rows.
   - **sm** — small cards, secondary promo blocks.
   - **md** — medium CTAs / tiles (~200–360 px wide), feature strips.
   - **lg** — hero-scale actions (e.g. 56 px height paired buttons), strong logo-format emphasis.
   - **xl** — large wide tiles (~350–420 px+), “medium-big” grid cards when intentionally using the mark.

3. **Replace** conflicting radius classes (`rounded-2xl`, `rounded-full`, old `rounded-[20px_20px_5px_20px]`, etc.) **only** on the targeted element — do not migrate unrelated components.

4. **Do not** apply logo corners to default content cards, FAQ rows, inputs, or nav — those stay on `--radius-card`, `--radius-input`, `--radius-pill` per the design doc.

5. After edits: ensure **`app/globals.css`** still defines **`--radius-logo-xs` … `--radius-logo-xxl`** and **`rounded-logo-xs` … `rounded-logo-xxl`**; align **08a** / **04** if the pattern changes. The **`--radius-logo-xxl`** token is defined alongside the other logo radii in **`app/globals.css`**.

## Reference

- Spec: `doc/design-doc/04-spacing-grid-layout.md` §8.1  
- Actions chapter: `doc/design-doc/08a-components-actions.md` (Primary — marketing variant)
