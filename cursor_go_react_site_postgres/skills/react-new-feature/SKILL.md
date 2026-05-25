---
name: react-new-feature
description: Adds a new frontend feature (component + hook + API service) following the project's feature-based structure. Use when adding a new dashboard widget or data-backed feature in React.
---

# Add a New React Feature

## Repo reality (this workspace)

This site lives under `app/` (`app/components/`, feature folders beside routes when needed). Do not use a `frontend/src/features/` layout — adapt all paths to the actual `app/` tree.

## Design alignment

Reuse `PrimaryCta`, `var(--color-*)`, and the `.type-*` typography utilities defined in `app/globals.css`. `doc/design-doc/` defines the role/spacing/motion contract.

> Note: paths below are templates from another project; in this repo, use `app/` equivalents (route segment + colocated component or `app/components/`).

**Repo note:** This workspace is a **Next.js App Router** site under **`app/`**. For a new **route/page**, use **`nextjs-new-route`** first. Use **this** skill for a **data-backed UI slice** co-located with features (adapt paths from `frontend/src/features/` to `app/...` or `app/components/...` as the project does).

Use this workflow when adding a new data-backed feature (e.g. a chart, table, or widget). Keeps components presentational and co-locates hook, types, and API call.

## Checklist

- [ ] Create feature folder under `frontend/src/features/<feature-name>/`
- [ ] Add types in `feature-name.types.ts` (request/response; mirror Go types if needed)
- [ ] Add API call in `feature-name.service.ts`: typed `fetch` or `api.get` returning `Promise<T>`; strip `data` from `ApiResponse<T>` before returning
- [ ] Add TanStack Query hook in `useFeatureName.ts`: `queryKey: ['feature-name', params]`, `queryFn` calling the service, `staleTime` (e.g. 30_000 for dashboards)
- [ ] Add presentational component that uses the hook; handle loading (skeleton), error (ErrorState), and success
- [ ] Compose in page or layout; wrap in ErrorBoundary so one broken widget does not crash the app

## File layout

```
features/
  feature-name/
    FeatureNameChart.tsx   (or Table, Card, etc.)
    useFeatureName.ts
    feature-name.types.ts
    feature-name.service.ts
```

## Rules

- Component receives only props needed for the query (e.g. filter); it does not receive raw data from parent
- Use skeleton for loading, not a spinner
- No `useEffect` for fetching — only React Query
- Add test file co-located: `useFeatureName.test.ts`, `FeatureNameChart.test.tsx` (MSW for API)

## After implementation

Recommend running the **verifier** subagent or the **testing** subagent to add/run tests.
