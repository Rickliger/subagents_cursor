# Skill: Add a Next.js App Router route

Short checklist for this repo (**Next.js App Router**, **`app/`**, **Tailwind v4**).

## 1. Pick the URL segment

- Decide path: e.g. `/sobre` → folder **`app/sobre/`**.

## 2. Add the page file

- Create **`app/sobre/page.js`** or **`page.tsx`** (match sibling routes).
- Default export the page component.
- Add **`metadata` export** (or **`generateMetadata`**) when SEO/titles matter — see Next docs in **`node_modules/next/dist/docs/`**.

## 3. Layout (only if needed)

- Add **`layout.js|tsx`** in that segment if this subtree shares a shell (nav wrapper, section spacing).
- Avoid duplicating root chrome already provided by **`app/layout`**.

## 4. Server vs client

- Start as a **Server Component** (no `"use client"`).
- Add **`"use client"`** only if you need **`useState`**, **`useEffect`**, browser APIs, or DOM event handlers.

## 5. Navigation

- Link from existing nav/footer using **`next/link`** with the new href.
- Prefer **`Link`** over raw `<a>` for internal navigation.

## 6. Styling

Use Tailwind layout utilities; pull semantic color, typography, spacing, elevation, motion, and z-order from `app/globals.css` (`var(--color-*)`, `--radius-*`, `--shadow-*`, `--duration-*`, `--ease-*`, `--z-*`) and the marketing typography utilities `.type-*`. Extend `globals.css` / `@theme` when a repeatable pattern lacks a token. Read `doc/design-doc/` (especially `03-typography.md`, `04-spacing-grid-layout.md`, `10-page-anatomy-flows.md`) for archetypes; avoid arbitrary `text-[Npx]` heading stacks.

### Marketing sections

Stacked marketing blocks should use `<MarketingSection>` from `app/components/MarketingSection.jsx` (or `marketingSectionClassName()` from `app/lib/sectionClasses.js` inside `"use client"` components). **Default:** `borderY` applies neutral top + bottom borders (`SECTION_BORDER_Y`). Pass `borderY={false}` for full-bleed heroes (`Hero`, `ProductStackedHero`, `bg-brand-electric` bands). Surface/padding presets: `surface` (`muted` | `brand-light` | `surface` | `transparent`), `padding` (`default` | `spacious` | `compact`).

## 7. Optional API for the route

- If the page needs a server endpoint, add **`app/api/.../route.js`** and call it from server or client per fetch/cache rules in the Next docs.

## 8. Verify

- Run the dev server and hit the new path; check metadata and layout nesting.

**Reminder:** Follow **`AGENTS.md`** — verify APIs against **`node_modules/next/dist/docs/`** for this project's Next version.
