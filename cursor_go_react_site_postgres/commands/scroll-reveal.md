---
description: >-
  Add or adjust GSAP scroll-reveal via ScrollReveal — agent must ask scope, direction,
  and options before editing code.
---

## When to use

The user ran **`/scroll-reveal`** (or invoked this command) to add the site’s scroll-reveal pattern to **new or existing UI**, or to change how it is applied.

## Canonical implementation

- **Component:** `app/components/ScrollReveal.jsx`
- **Styles:** `.scroll-reveal-init` in `app/globals.css` (opacity-only shell; motion comes from GSAP)
- **Usage:** `import ScrollReveal from "./components/ScrollReveal";` (adjust relative path from the file you edit, e.g. `../components/ScrollReveal` from `app/clt/page.js`)

```jsx
<ScrollReveal className="tailwind layout classes" entryFrom="bottom">
  …children…
</ScrollReveal>
```

**Props (optional unless user specifies otherwise):**

| Prop | Default | Notes |
|------|---------|--------|
| `entryFrom` | `"bottom"` | `"bottom"` \| `"top"` \| `"left"` \| `"right"` \| `"none"` (fade only) |
| `y` | `14` | Offset in **px** (vertical for top/bottom, horizontal magnitude for left/right) |
| `duration` | `0.58` | Tween length (seconds) |
| `delay` | `0.15` | Delay before tween (seconds) |
| `className` | `""` | On the wrapper `div` — use for flex/grid/width so layout does not break |
| `id` | — | Rare; e.g. anchor id when needed |

**`entryFrom` vs plain language (use this mapping when the user answers):**

| User phrase (examples) | `entryFrom` |
|-------------------------|-------------|
| Up, from below, rises | `bottom` |
| Down, from above, drops | `top` |
| From the left, slides right | `left` |
| From the right, slides left | `right` |
| Fade only, no slide | `none` |

If the user says **“reveal to the right”**, clarify: they might mean motion **toward** the right → usually `entryFrom="left"`. If unsure, **ask once** with two options tied to `entryFrom` values.

Where possible, align default durations and easings to `--duration-base` / `--duration-fast` and `--ease-standard` (defined in `app/globals.css`). `doc/design-doc/07-motion.md` is normative; document any deliberate deviation that keeps raw seconds.

## Mandatory: ask before coding

**Do not** edit files until the user has answered (use **AskQuestion** when the tool is available; otherwise ask in chat):

1. **Scope**
   - **Whole page:** wrap each major block (e.g. per `<section>`, or logical groups inside `<main>`), mirroring patterns on `/`, `/fgts`, `/clt`. **Or**
   - **Specific element(s):** which **route** (`app/.../page.js`) or **component** (`app/components/...`), and what to wrap (section title, card grid, single card, etc.). If vague, ask for file path or paste JSX.

2. **Direction / motion** (one choice, or per-block if they want mixed)

   Offer: **Bottom (default)** · **Top** · **Left** · **Right** · **Fade only** — map to `entryFrom` as in the table above.

3. **Overrides (optional)**

   - Change **offset** (`y` prop), **duration**, or **delay**? Defaults are `14`, `0.58`, `0.15` unless the user asks different numbers.

4. **Exclusions (optional)**

   - Confirm **not** wrapping: sticky **header**, **above-the-fold hero** / LCP-critical media, or tiny chrome — unless the user explicitly wants those animated.

## After answers

1. Import `ScrollReveal` and wrap the agreed nodes; pass `entryFrom` (and overrides only if requested).
2. Keep **`"use client"`** boundaries in mind: importing `ScrollReveal` into a Server Component is allowed; the **imported** module is a client component.
3. Prefer **one wrapper per logical block** (not double-wrapping existing `ScrollReveal` without reason).
4. Run **`npm run build`** (or project lint) if you touched many files.

## Do not

- Reintroduce `@gsap/react` `useGSAP` with a changing dependency array length (causes React warnings).
- Add scroll-reveal to **every** leaf node — prefer section-level wrappers.
- Ship a different animation library for the same effect without the user asking.
