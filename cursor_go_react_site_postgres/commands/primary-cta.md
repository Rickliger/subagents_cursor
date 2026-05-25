---
description: >-
  Use shared PrimaryCta (green CTA) — link vs button, client PrimaryCtaAction for router / new tab, tone, layout, icon, then edit PrimaryCta.jsx or usages.
---

## When to use

User ran **`/primary-cta`** to add or change the **canonical green CTA** used across pages.

## Canonical design pattern (green CTA)

Unless design specifies otherwise, **primary green CTAs** on this site use the **same shell as the FGTS hero “Fale conosco”**:

| Piece | Value |
|--------|--------|
| Element | **`PrimaryCtaAction`** (WhatsApp / `window.open`) or **`PrimaryCta`** with **`href`** when a real link is required |
| **`layout`** | **`default`** → `rounded-logo-lg px-7 py-4 text-base` (see `layoutClass` in `PrimaryCta.jsx`) |
| **`tone`** | **`onBrandHero`** → white `focus-visible` ring (matches blue / photo hero surfaces) |
| Width | Typical: **`className="w-full xl:w-auto"`** (hero); in narrower columns **`w-full sm:w-auto`** or **`w-full max-w-sm sm:mx-auto sm:w-auto`** (FGTS bottom band) |
| Icon | WhatsApp: **`icon={<WhatsappIcon className="h-6 w-6 shrink-0" />}`** (same size as hero) |

**Do not** ship one-off WhatsApp anchors with **`bg-[#25D366]`**, **`pillMobile`**, or stacked **`min-h-*` / `rounded-2xl`** classes that duplicate `PrimaryCta` — use **`PrimaryCtaAction`** + **`openUrl`** so spacing, hover (`interactive-cta`), and focus stay consistent.

On **light gray / white** sections only, if the white focus ring is hard to see, **`tone="onLight"`** is allowed — still keep **`layout="default"`** unless design changes the shape.

## Source of truth

- **`app/components/PrimaryCta.jsx`** — visuals + semantics:
  - **`href`** internal path → **`<Link>`** (prefetch, SEO, middle-click).
  - **`href`** `http(s)://...` → **`<a>`** (external; pass `target` / `rel` if needed).
  - **No `href`** → **`<button>`** (pass **`onClick`**, **`type="submit"`** when appropriate).  
  Styling is driven by **`greenCore`**, **`toneClass`**, and **`layoutClass`** in that file.

- **`app/components/PrimaryCtaAction.jsx`** — **client** wrapper that always renders **`PrimaryCta`** as a **`<button>`**:
  - **`navigateTo="/fgts"`** → `router.push(...)` (home hero product CTAs).
  - **`openUrl="https://..."`** → `window.open(..., "_blank")` with `opener` cleared (e.g. WhatsApp on `/fgts` hero).

Use **`PrimaryCtaAction`** from **Server Components** when you need navigation or a new tab but want a **button** (same shell as `PrimaryCta`). Pass only serializable props plus optional **`icon`** children from the server.

Tokens (in `app/globals.css`): `--color-cta`, `--color-cta-hover`, `--color-cta-tint`. Canonical label typography is `text-base font-semibold` (16px) on `layout="default"`; never `font-bold`. Reference docs: `doc/design-doc/08a-components-actions.md` and `doc/design-doc/14-handoff-and-tokens.md`.

## Usage examples

**Link (internal route, default for SEO-critical nav):**

```jsx
import PrimaryCta from "./components/PrimaryCta"; // adjust path

<PrimaryCta href="/fgts" tone="onBrandHero" layout="default" className="w-full xl:w-auto" iconSrc="/icons/cofrinho.svg">
  Saque Aniversário FGTS
</PrimaryCta>
```

**Button + in-app navigation (matches current home hero):**

```jsx
import PrimaryCtaAction from "./components/PrimaryCtaAction";

<PrimaryCtaAction navigateTo="/fgts" tone="onBrandHero" layout="default" className="w-full xl:w-auto" iconSrc="/icons/cofrinho.svg">
  Saque Aniversário FGTS
</PrimaryCtaAction>
```

**Button + external URL in new tab (matches current FGTS hero “Fale conosco”):**

```jsx
import PrimaryCtaAction from "./components/PrimaryCtaAction";

<PrimaryCtaAction openUrl={WHATSAPP_URL} tone="onBrandHero" layout="default" className="w-full max-w-md" icon={<WhatsappIcon className="h-6 w-6 shrink-0" />}>
  Fale conosco
</PrimaryCtaAction>
```

## Ask before coding (AskQuestion when available)

1. **Element choice**
   - **`<Link>` / `<a>`** → **`PrimaryCta`** with **`href`** (internal vs external auto-detected).
   - **`<button>`** + **`router.push`** or **`window.open`** → **`PrimaryCtaAction`** with **`navigateTo`** or **`openUrl`** (do not hand-roll `router.push` in the page unless there is a one-off reason).

2. **Surface / focus ring** → **`tone`**: `onBrandHero` (white ring on blue hero) or `onLight` (brand ring on light backgrounds).

3. **Shape / density** → default is **`layout="default"`** for primary green CTAs (see **Canonical design pattern**). Use **`pillSm`** / **`pillMobile`** only when design explicitly asks for a different shape.

4. **Label** (`children`) and icon: **`iconSrc`** (public path, inverted in `PrimaryCta`) or **`icon`** (React node, e.g. inline SVG).

## Trade-offs (buttons)

**`PrimaryCtaAction`** (and any **`<button>`** nav) drops **link** affordances: no native prefetch, no middle-click to same-tab URL, crawlers do not follow as a normal anchor. Prefer **`PrimaryCta` + `href`** when SEO or link UX matters unless the product explicitly wants a button.

## Do not

- Replace **`PrimaryCtaAction`** with ad-hoc **`useRouter` + `PrimaryCta`** wrappers per page — extend **`PrimaryCtaAction`** or **`PrimaryCta`** once instead.
- Use **`router.push`** on a fake **`<a href="#">`** for primary CTAs — pick **`href`** + **`PrimaryCta`** or **`PrimaryCtaAction`** intentionally.
