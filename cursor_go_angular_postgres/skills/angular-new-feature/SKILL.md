---
name: angular-new-feature
description: Adds a new frontend feature (routed page + components + service) following the project's Angular feature structure. Use when adding a new dashboard widget or data-backed feature in Angular.
---

# Add a New Angular Feature

Use this workflow when adding a new data-backed feature (e.g. a chart, table, or widget). Keep **presentational** pieces dumb; put HTTP and mapping in an **injectable service**.

## Checklist

- [ ] Create folder under `frontend/src/app/features/<feature-name>/`
- [ ] Add types in `feature-name.types.ts` (request/response DTOs; mirror Go models if needed)
- [ ] Add `feature-name.service.ts` with `HttpClient` methods returning `Observable<T>`; map `ApiResponse<T>` to domain types in the service (strip envelope there)
- [ ] Add **standalone** route entry (e.g. `feature-name.routes.ts`) or register routes in the parent feature module the project uses
- [ ] Add **smart** container + **dumb** child components as needed; use `async` pipe or explicit RxJS with teardown
- [ ] Handle loading (skeleton), error state, and empty data in the template
- [ ] Compose in a parent layout or dashboard; use an error boundary pattern the app already has (e.g. dedicated error component or route-level handler)

## File layout (typical)

```
features/
  feature-name/
    feature-name.routes.ts
    feature-name-page.component.ts
    feature-name-page.component.html
    feature-name-page.component.scss
    feature-name-chart.component.ts
    feature-name-chart.component.html
    feature-name.service.ts
    feature-name.types.ts
```

## Rules

- **Do not** call `HttpClient` from every component — centralize in `FeatureNameService`
- Use **skeleton** for loading, not only a spinner
- Prefer **`takeUntilDestroyed()`** or **`async` pipe** over leaked subscriptions
- Co-locate tests: `feature-name.service.spec.ts`, `feature-name-page.component.spec.ts`

## After implementation

Recommend running the **verifier** subagent or the **testing** subagent to add/run tests.
