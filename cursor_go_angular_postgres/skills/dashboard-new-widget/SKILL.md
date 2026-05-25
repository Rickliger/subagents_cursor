---
name: dashboard-new-widget
description: Adds a new dashboard widget (KPI card or chart) following the UI design system. Use when adding a KPI strip item or a chart/widget to a dashboard page with correct hierarchy and spacing.
---

# Add a New Dashboard Widget

Use this workflow when adding a KPI or chart widget so it follows clarity and hierarchy rules.

## Principles (apply every time)

1. **One primary action per view** — the new widget is either the dominant element or a supporting one. If supporting, keep it smaller and less saturated.
2. **Answer one question** — the widget must answer a specific user question. If it's "nice to know", put it on a detail page instead.
3. **Breathing room** — minimum `p-4` inside cards, `gap-4` between grid cells.
4. **Hierarchy by size/weight** — not by color. One primary accent; use grays for structure.

## KPI card

- Props: `label`, `value`, optional `change` (value, direction, good), optional `icon`
- Style: rounded border, white background, label `text-sm text-gray-500`, value `text-3xl font-semibold`
- Place in a strip with max 4 KPIs per row; use existing `KPIStrip` or grid.

## Chart widget

- Choose type by data: trend over time = line; part-to-whole = donut (max 5 segments); comparison = grouped bar.
- Use project chart defaults: muted axis labels, simple tooltip, CHART_COLORS order; max 5 series per chart.
- Loading: skeleton that mirrors chart shape (e.g. bar placeholder), not spinner.
- Empty state: icon + short message (e.g. "No data for this period").

## Layout

- Primary chart: `col-span-full`, fixed height (e.g. `h-80`).
- Supporting grid: `grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4`. Never more than 4 columns.

## After implementation

Ensure the new widget is isolated from crashing siblings (e.g. dedicated error UI or route-level error handler in Angular) and that the page still has one dominant element. If the dashboard feels crowded, move a metric to a detail view instead of adding another row.
