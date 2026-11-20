# Data grid in 2.x

Use this reference in a 2.x project when a screen needs a dense, editable, spreadsheet-style grid:
search, typed filters, multi-column sort, group by, pinned and resizable columns, inline editing,
ranges, fill, Excel-compatible copy and paste, undo, and CSV or xlsx export.

## It is example code, not an export

`@nesra/ui` has no `DataGrid` export. `Table` stays presentational. The Data grid is example code in
the design system repository at `apps/docs/src/examples/table/data-grid/`, documented on the Table
docs page, section Data grid (`/components/table/#data-grid`, also `/components/table.md`). A
step-by-step build is at `/components/data-grid-tutorial/`. Copy it into the app and adapt it there.

- Never write `import { DataGrid } from "@nesra/ui"` or a `@nesra/ui/...` subpath for it.
- Do not add another grid library for this. If the app needs something the example does not do,
  change the copied files.
- For a read-only list, compose `Table` parts instead: the grid is heavier than most tables need.

## Copy and render

1. Copy `data-grid.tsx`, `use-data-grid.ts`, `columns.ts`, `export.ts`, and `motion.tsx` into the app, for example
   `src/components/data-grid/`. `data.ts` holds only the Renewals demo defaults: remove the
   `renewalColumns` and `makeRenewals` defaults at the top of `DataGridInner` in `data-grid.tsx`,
   then delete it.
2. Dependencies are `@nesra/ui` (it uses `Table`, `Popover`, `DatePicker`, and other parts),
   `lucide-react`, and React. The xlsx writer needs no package.
3. The app's Tailwind setup must scan the copied folder.

```tsx
import { useState } from "react";
import { DataGrid } from "./data-grid/data-grid";
import type { GridColumn, GridRow } from "./data-grid/columns";

const columns: GridColumn[] = [
  { key: "invoice", label: "Invoice", type: "text", width: 140, pinned: true, editable: false },
  { key: "customer", label: "Customer", type: "text", width: 200, pinnable: false },
  { key: "status", label: "Status", type: "select", width: 130, options: ["Draft", "Sent", "Paid", "Overdue"] },
  { key: "amount", label: "Amount", type: "currency", width: 140, decimals: 2 },
  { key: "due", label: "Due date", type: "date", width: 150 },
];

export function Invoices({ initialRows }: { initialRows: GridRow[] }) {
  const [rows, setRows] = useState(initialRows);
  return <DataGrid title="Invoices" columns={columns} rows={rows} onRowsChange={setRows} />;
}
```

## Columns

Each `GridColumn` has `key`, `label`, `type`, and `width` (required), plus optional `options`,
`decimals`, `editable`, `aggregate`, `groupable`, `pinned`, and `pinnable`.

- `type` is `text`, `number`, `currency`, `percent` (stores 12.5 for 12.5%), `select` (one of
  `options`), or `date` (ISO day, such as `2026-01-31`).
- `pinned: true` starts a column pinned. If no column sets `pinned`, the first column starts pinned.
  If any column sets it, only those columns start pinned. Pinned columns use at most half the grid
  width.
- `pinnable: false` removes Pin and Unpin from the column's header menu, and the Columns menu never
  marks it Pinned.
- Columns are read once, on mount. To change them, remount the grid with a new `key`.
- Every row needs a unique, stable string `id`. Empty cells are `null`.

## Data

- `rows` passes the app's data in. `onRowsChange(rows)` receives the full row list after every edit,
  paste, fill, cut, clear, delete, undo, redo, and Reset. It is not called on mount.
- Passing back the array from `onRowsChange` changes nothing. A different array, such as a fresh
  server response, loads as new data: the undo history and checked rows start over, and the view
  (sort, filters, columns, density) stays.
- `onSelectionChange(ids)` reports checked rows. `title` names the grid and the export file.
  `maxHeight` sets the scroll area height. `defaultGroupBy` (array of column keys, outermost first)
  sets the starting grouping, and Reset returns to it.
- Save in `onRowsChange`, for example with `fetch`. To send only changes, compare the new rows with
  the previous ones by `id`.

## Behavior to expect

- Date cells: Alt+Down or the calendar button opens a calendar in a popover. Picking a day saves the
  ISO value and ends the edit. Typing an ISO date still works. Esc closes the calendar and keeps
  editing.
- Nested group by: group by more than one column from the header menu. A "Grouped by" bar shows the
  levels, moves a column out one level, or stops grouping. Each level shows a subtotal. The grid
  becomes a `treegrid` with `aria-level` and `aria-expanded`. Paging counts data rows, and group
  headers repeat on each page. xlsx export keeps the groups as an outline with `SUBTOTAL` formulas;
  CSV has data rows only.
- Motion: sorted rows slide to their new place, the active-cell and range outlines glide, and totals
  and counts count up. Screen readers get only the final value. All motion stops when the user
  prefers reduced motion. Keep `motion.tsx` with the other files.

## Filter syntax to give users

Text `olivia`, `=exact`, `!excluded`. Numbers `> 100`, `>= 5`, `!= 0`, `10..50`. Dates
`> 2026-01-31`, `2026-03`, `2026..2027`. Select columns use a checklist ("3 of 4").

## Limits

No row virtualization (pages of 25, 50, or 100 rows), and no calendar in the filter row: use typed
date filters there.
