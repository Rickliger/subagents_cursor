---
name: go-new-endpoint
description: Adds a new REST endpoint following project conventions. Use when creating a new API resource (handler + service + repository interface). Covers Go backend only; for SQL or migrations use the database subagent.
---

# Add a New Go API Endpoint

Use this workflow when adding a new API resource. Ensures handler/service/repository layering and consistent naming.

If the endpoint is part of a planned initiative, read the relevant spec in [`doc/specs/`](../../doc/specs/) and status in [`doc/00-INDEX.md`](../../doc/00-INDEX.md) first. If the task cites a backlog item (`Backlog BNNN`), read that file in [`doc/backlog/`](../../doc/backlog/) for acceptance criteria.

## Checklist

- [ ] Define request/response types in `internal/model/` (or existing domain package)
- [ ] Add repository interface method in the appropriate repo interface
- [ ] Implement service method that calls the repository; add validation and error wrapping
- [ ] Add thin handler: bind input (query/path/body), call service, respond with JSON or Problem Details on error
- [ ] Register route in `main.go` or route group (e.g. `r.Get("/v1/resource", h.HandleGetResource)`)
- [ ] Use `respondJSON` and `respondError`; set context timeout for heavy operations

## Naming

| Layer | Pattern | Example |
|-------|---------|--------|
| Handler | `HandleGetX`, `HandleCreateX` | `HandleGetDashboard` |
| Service | `GetX(ctx, filter)` | `GetDashboard(ctx, filter)` |
| Repository | `FindX(ctx, ...)` | `FindDashboardMetrics(ctx, filter)` |

## Route and envelope

- Path: `/{version}/{resource}` e.g. `/v1/dashboard`
- Response: `{ "data": <T>, "error": null, "meta": {...} }`
- Errors: RFC 7807 Problem Details, never expose stack traces or raw DB errors

## After implementation

Recommend running the **verifier** subagent or writing a quick handler test (httptest + mock service).
