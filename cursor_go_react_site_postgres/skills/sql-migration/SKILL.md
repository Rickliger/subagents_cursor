---
name: sql-migration
description: Creates a new PostgreSQL migration with correct naming, up/down, and index conventions. Use when adding or changing tables, columns, or indexes. The agent never runs migrations — only creates files and explains; the user executes.
---

# Create a SQL Migration

Use this workflow when adding or changing schema. Keeps migrations safe (CONCURRENTLY, up/down) and consistent with project conventions.

**The agent never runs migrations.** Create the migration files and then explain in plain language what the migration does (what it creates, changes, or drops; what the user should expect). The user always executes migrations themselves.

## Naming

- Numbered files: `001_description.sql`, `002_another_change.sql`
- Use snake_case in descriptions: `add_metrics_table`, `add_index_orders_created_at`

## File layout

One directory or pattern for ups/downs:

- `migrations/NNN_description.up.sql`
- `migrations/NNN_description.down.sql`

Or single file with `-- up` / `-- down` comments if your migrator supports it.

## Up migration

1. **Tables**: `CREATE TABLE` with explicit columns; `UUID PRIMARY KEY DEFAULT gen_random_uuid()` for ids; `TIMESTAMPTZ` for timestamps; `NOT NULL` and `REFERENCES` where appropriate.
2. **Indexes**: Always `CREATE INDEX CONCURRENTLY idx_<table>_<columns> ON <table> (...)` to avoid long table locks.
3. **Index rules**: Index every FK; composite `(entity_id, recorded_at DESC)` for time-series; partial index for high-selectivity filters.
4. **Columns**: Prefer adding new columns and backfilling over in-place ALTER that can lock or fail.

## Down migration

1. Drop in reverse order: indexes first if needed, then tables. Use `DROP TABLE IF EXISTS` and match the up migration so re-running is safe.
2. If the up adds FKs, down must drop or alter dependents first; never leave broken references.

## Do not

- Do not run migrations from application startup — use a dedicated migration step (e.g. CI or deploy script).
- Do not use `ALTER COLUMN` to a breaking type change without a multi-step migration (add column, backfill, drop old).
- Do not use `SELECT *` in any application query; migrations may create tables with many columns, but repos should list columns explicitly.

## After creating the migration

1. **Explain for the user:** In plain language, describe what the up migration does (e.g. "Creates table X with columns … and index …") and what the down does. Mention any run order or dependency.
2. Tell the user they need to run the migration themselves (e.g. with their migrate tool or psql).
3. Optionally recommend invoking the **database** subagent to implement or update the repository method that uses the new schema.
