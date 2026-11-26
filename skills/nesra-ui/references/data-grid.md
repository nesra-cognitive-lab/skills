# Data grid in 2.x

Use this reference in a 2.x project when a screen needs a dense, spreadsheet-style grid: search,
typed filters, multi-column sort, group by, pinned and resizable columns, inline editing, ranges,
fill, Excel-compatible copy and paste, undo, and CSV or xlsx export. The same grid also has a
read-only mode (`mode="view"`) with row click, and can load rows on scroll instead of paging.

## It is example code, not an export

`@nesra/ui` has no `DataGrid` export. `Table` stays presentational. The Data grid is example code in
the design system repository at `apps/docs/src/examples/table/data-grid/`, documented on the Table
docs page, section Data grid (`/components/table/#data-grid`, also `/components/table.md`). A
step-by-step build is at `/components/data-grid-tutorial/`. Copy it into the app and adapt it there.

The tutorial order: steps 2 to 9 build the editable Invoices grid (columns, types, editing,
search and filters, pinning, ranges and undo, totals and grouping, export). Step 10 makes it
view-only with row click (`mode="view"`, `onRowClick`). Step 11 loads rows from a server with the
pagination (`onLoadMore`, `loadMoreOn="pagination"`, `hasMore`, `totalRows`, `totals`). Step 12 is
the finished editable grid (Storybook `Tutorial result`). Point a user to the step that matches
the screen they build.

- Never write `import { DataGrid } from "@nesra/ui"` or a `@nesra/ui/...` subpath for it.
- Do not add another grid library for this. If the app needs something the example does not do,
  change the copied files.
- For a short read-only list without the toolbar, compose `Table` parts instead. For a read-only
  list that needs the grid's search, filters, sort, columns, density, export, and totals, use the
  grid with `mode="view"` (see [Edit or view mode](#edit-or-view-mode)).

## Horizontal scroll button and fade

The Data grid keeps its own scroll container (vertical scroll with a sticky header, pinned columns,
and the active cell outline), so it does not draw the Figma Scroll fade and round Scroll button. A
plain `Table` in a card does not need the example: wrap it in `HorizontalScroll` from
`@nesra/ui/metric` (`insetStart="auto"` keeps the pinned columns clear). Nesra Scale's Overview,
Shop and Ads tables use it that way. Do not add a scroll prop to the Table or edit the example for
this.

## Copy and render

1. Copy `data-grid.tsx`, `use-data-grid.ts`, `columns.ts`, `export.ts`, and `motion.tsx` into the app, for example
   `src/components/data-grid/`. `data.ts` holds only the Renewals demo defaults: remove the
   `renewalColumns` and `makeRenewals` defaults at the top of `DataGridInner` in `data-grid.tsx`,
   then delete it.
2. Dependencies are `@nesra/ui` (it uses `Table`, `Popover`, `DatePicker`, `LoadingIndicator`,
   and other parts), `lucide-react`, and React. The xlsx writer needs no package.
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
  sets the starting grouping, and Reset returns to it. `defaultDensity` (`compact` default,
  `standard`, `comfortable`) sets the starting row height (32, 40, 48px).
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
- Date filters: the calendar button in a date filter cell, or Alt+Down there, opens a calendar.
  Above the days, a comparison row: On (`=`, default), After (`>`), Before (`<`), On or after
  (`>=`), On or before (`<=`). Picking writes `2026-01-31` or `> 2026-01-31`. A typed filter opens
  on its day and comparison. A range opens on its first day and a pick replaces it, so ranges are
  typed. Typed filters work as before.
- Motion: sorted rows slide to their new place, the active-cell and range outlines glide, and totals
  and counts count up. Screen readers get only the final value. All motion stops when the user
  prefers reduced motion. Keep `motion.tsx` with the other files.

## Edit or view mode

`mode` is `"edit"` (default) or `"view"`. Pick by what the user does with the data:

- **Edit** (`mode` omitted): the user changes values. Spreadsheet behavior with a cell outline,
  range, fill, paste, undo. Matches Figma Table `Example=Data grid editing`. Unchanged from before.
- **View** (`mode="view"`): the user reads, sorts, filters, exports, or opens a row, but never
  changes data (reports, logs, lists that open a detail page). Matches Figma Table
  `Example=Data grid` (node 15679:18397) and `Data grid dark` (15679:18403).

| Feature | `mode="edit"` (default) | `mode="view"` |
| --- | --- | --- |
| Edit cells (double-click, Enter, F2, typing) | Yes | No |
| Cell outline (active cell) and range outline | Yes | No |
| Fill handle, Cmd/Ctrl+D, paste, cut, Delete | Yes | No |
| Undo and Redo (toolbar and keys), Reset | Yes | No |
| Status bar (cell, Count, Sum, Average) | Yes | No |
| Bulk actions on checked rows | Copy, Export, Delete (Undo toast) | Copy, Export |
| Keyboard focus | One cell (`aria-activedescendant`) | One row (roving `tabindex`), focus ring |
| Row click (`onRowClick`) | Ignored: a click picks a cell | Click, Enter, Space |
| Load more (`onLoadMore`) | Yes | Yes |
| Search, filters, sort, columns, density, export, pin, resize, group by, totals, check boxes | Yes | Yes |

`editable: false` on a column is not view mode: it skips that column in edits, paste, and fill but
still shows the cell outline. Use `mode="view"` for a whole read-only grid.

View mode keys: Tab reaches the current row (one row is tabbable), Up and Down move between rows,
Home and End go to the first and last row, Page Up and Page Down move by a screen, Enter and Space
activate the row (`onRowClick`), Shift+Space checks the row, Escape clears checked rows. Text in
cells can be selected and copied with the browser's copy.

```tsx
import { DataGrid } from "./data-grid/data-grid";

<DataGrid
  title="Projects"
  columns={columns}
  rows={rows}
  mode="view"
  defaultDensity="standard" // Figma view frame: 40px rows
  onRowClick={(row) => navigate(`/projects/${row.id}`)} // your router
/>;
```

## Row click

`onRowClick(row, event)` works only in view mode. When set, rows get a pointer cursor, and a click
or Enter or Space on the focused row calls it with the `GridRow` and the React mouse or keyboard
event (`RowClickEvent`). Open the detail page, a `Sheet`, or a `Dialog` there. Figma draws no row
target, so choose what the product needs.

Accessibility rules the grid already follows (keep them when you change the copy):

- Clicks on the check box gutter (the check box and the row number cell) never call it.
- Buttons, links, inputs, menus, and elements with `data-row-click-ignore` inside a cell keep their
  own click. Put `data-row-click-ignore` on any custom interactive element you add to a cell.
- A click that ends a text selection and Shift+Space (check row) do not call it.
- The focused row shows a 2px `focus-ring` on its edges (Figma draws no row focus, so the library
  focus ring is used). Never remove it: the row is focusable.
- The table has `role="grid"` (`treegrid` when grouped) and `aria-readonly="true"`.

## Load more

`onLoadMore` loads the next block from the server. `loadMoreOn` picks where it runs:

- `"scroll"` (default): `onLoadMore` replaces the pagination with Figma Table footer
  `Type=Load on scroll` (Figma `Example=Data grid filtered`, node 15679:18399): the next block
  loads automatically when the end of the scroll area comes near. There is no Load more button in
  Figma. Keyboard users load more with Down, Page Down, or Cmd/Ctrl+End on the last loaded row.
- `"pagination"`: the Figma `Type=Pagination` footer stays (node 15573:8552). The right-side
  buttons load: see [Load more from the pagination](#load-more-from-the-pagination).

Use `scroll` for feeds and long lists people skim. Use `pagination` when people page through
records, jump to a page, or need "Page 2 of 8".

| Prop | Type | Default | Meaning |
| --- | --- | --- | --- |
| `onLoadMore` | `() => void \| Promise<void>` | none | Load the next block and append it to `rows`. |
| `hasMore` | `boolean` | `true` | `false` stops loading and shows "All N rows loaded". |
| `loadingMore` | `boolean` | pending state of the returned promise | Shows the spinner and "Loading more rows". |
| `totalRows` | `number` | none | Server count: "50 of 10,000 rows loaded" and "24 of 128 rows". |
| `totals` | `Partial<Record<string, number \| null>>` | none | Server totals by column key (Figma totals cover every row that passes the filters, not only loaded rows). |
| `loadMoreOn` | `"scroll" \| "pagination"` | `"scroll"` | Load near the end of one scrolling list, or from the pagination buttons. |

```tsx
import { useEffect, useState } from "react";
import { DataGrid } from "./data-grid/data-grid";
import type { GridRow } from "./data-grid/columns";

const PAGE = 50;

async function fetchPage(offset: number) {
  const response = await fetch(`/api/projects?offset=${offset}&limit=${PAGE}`);
  if (!response.ok) throw new Error("Could not load projects");
  return (await response.json()) as { rows: GridRow[]; total: number };
}

export function Projects() {
  const [rows, setRows] = useState<GridRow[]>([]);
  const [total, setTotal] = useState<number>();

  useEffect(() => {
    void fetchPage(0).then((page) => {
      setRows(page.rows);
      setTotal(page.total);
    });
  }, []);

  return (
    <DataGrid
      title="Projects"
      columns={columns}
      rows={rows}
      mode="view"
      onLoadMore={async () => {
        const page = await fetchPage(rows.length);
        setRows((current) => [...current, ...page.rows]); // append, never replace
        setTotal(page.total);
      }}
      hasMore={total === undefined || rows.length < total}
      totalRows={total}
    />
  );
}
```

- Append: pass an array that starts with the current rows (the same objects, in order). The grid
  then keeps sort, filters, search, grouping, pinned and hidden columns, scroll, checked rows, and
  (edit mode) the undo history. Any other array loads as new data and starts over.
- Sort, filters, and search run on loaded rows, so new rows join in sorted order. For server-side
  filtering, fetch again with the filter and pass a new array.
- The footer and the table get `aria-busy="true"` while loading, and the status text is announced
  politely. The spinner itself is hidden from screen readers.
- A failed promise ends the loading state. Show your own error; scrolling to the end tries again.

### Load more from the pagination

With `loadMoreOn="pagination"`, Next (›), Last (»), or a page number past the loaded rows calls
`onLoadMore` until that page is full (or `hasMore` is `false`), then opens the page. Pages already
loaded open at once. Figma draws no loading state for the Pagination footer, so the grid adds one
next to Rows per page.

```tsx
<DataGrid
  title="Invoices"
  columns={columns}
  rows={rows}
  mode="view"
  loadMoreOn="pagination"
  onLoadMore={async () => {
    const page = await fetchInvoices(rows.length); // fetch(`/api/invoices?offset=...&limit=25`)
    setRows((current) => [...current, ...page.rows]); // append, never replace
  }}
  hasMore={rows.length < total} // false: Next and Last disabled on the last page
  totalRows={total} // page count: "Page 1 of 8"
  totals={{ amount: serverAmountSum }} // optional server totals
/>
```

- **Page count:** with `totalRows` and no filter or search, every server page ("of 8" for 200 rows
  at 25 per page). Otherwise the loaded pages, plus one while `hasMore` is `true`.
- **Loading state:** the `LoadingIndicator` spinner (hidden from screen readers) and the visible
  text "Loading page 2" on the left of the pagination; `aria-busy="true"` on the pagination `nav`
  and the table; the text is announced politely. After the rows arrive, the page opens and a
  visually hidden status reads "Page 2 of 8, 50 of 200 rows loaded" (also announced).
- **Focus:** Next is not disabled while loading, so keyboard focus stays on it. Clicks and Enter
  during a load are ignored. After the load, focus is still on Next and pressing it again goes on.
- **End:** `hasMore={false}` on the last page disables Next and Last; the status adds "all rows
  loaded".
- **Last (») and page numbers** load every block up to that page, one `onLoadMore` call at a time.
  With a filter or search active, a page fills only with matching loaded rows, so it can take
  several blocks. A load that adds no rows stops on the last page that has rows.
- **Rows per page also loads.** Choosing a size goes to page 1. When page 1 at the new size needs
  rows that are not loaded (25 loaded, 50 chosen) and `hasMore` is `true`, the grid calls
  `onLoadMore` until page 1 is full or `hasMore` is `false`, with the same spinner, place,
  `aria-busy`, and polite announcement as Next. The visible text is "Loading more rows" (Figma's
  Loading more text; "Loading page 1" would be wrong because page 1 already shows). When done, the
  visually hidden status announces "Showing 50 rows per page, 50 of 128 rows loaded" until the next
  page change.
- **Enough rows or no more rows:** a smaller size, a size already loaded, or `hasMore={false}`
  loads nothing and shows no spinner; the page shows the rows there are and the status announces
  the new size. With 70 server rows and 100 chosen, the grid loads until `hasMore` is `false` and
  shows 70.
- **During a load,** a Rows per page change is ignored like a Next click: the select keeps its value
  and focus returns to it. Rows per page never changes the block size the server sends.
- Down, Page Down, and Cmd/Ctrl+End do not load in this mode. Load on scroll (`"scroll"`) has no
  Rows per page (the Figma Load on scroll footer shows only the status); do not add one.

| Action | Loaded rows fill the page | Rows missing, `hasMore` | Rows missing, no `hasMore` |
| --- | --- | --- | --- |
| Next, Last, or a page number | Opens the page | "Loading page N", then opens it | Opens the last page with rows |
| Rows per page | Page 1 at the new size | Page 1, "Loading more rows" until full | Page 1 with the rows there are |

- Keep these behaviors when you edit the copy: do not disable Next or the Rows per page select while
  loading (focus would be lost), keep the announced status texts, and keep `aria-busy`.

## Filter syntax to give users

Text `olivia`, `=exact`, `!excluded`. Numbers `> 100`, `>= 5`, `!= 0`, `10..50`. Dates
`> 2026-01-31`, `2026-03`, `2026..2027`. Select columns use a checklist ("3 of 4").

## Limits

No row virtualization (pages of 25, 50, or 100 rows, or every loaded row with `onLoadMore`: keep
loaded lists to a few thousand rows). No Load more button (Figma loads on scroll; use
`loadMoreOn="pagination"` for paging). No server-side
sort or filter hooks. Cells show formatted values only (no avatars or badges), in both modes. The
filter calendar picks one day: type ranges (`2026-01-01..2026-03-31`), months (`2026-03`), and
years (`2026`).

## Where to look

- Docs: `/components/table/#data-grid`, sections Modes (`#modes`), View-only mode
  (`#view-only-mode`), Row click (`#row-click`), Load more (`#load-more`), and Load more from the
  pagination (`#load-more-from-the-pagination`, including Rows per page and the action table).
- Tutorial: `/components/data-grid-tutorial/`, step 10 View-only mode and row click
  (`#10-view-only-mode-and-row-click`), step 11 Load more from the server with the pagination
  (`#11-load-more-from-the-server-with-the-pagination`), step 12 The finished grid.
- Storybook `Components/Table/Data grid`: `View only`, `View only (Dark)`, `Row click`,
  `Row click (Dark)`, `Load more`, `Load more (Dark)`, `Load more (pagination)`,
  `Load more (pagination, Dark)` (try Next, Last, and Rows per page 50 or 100); edit mode in `Default`, `Dark`, `Custom columns`, and
  `Tutorial result`.
- Example files: `data-grid-view-only.tsx`, `data-grid-load-more.tsx`,
  `data-grid-load-more-pagination.tsx`, `tutorial-step-10-view-only.tsx`,
  `tutorial-step-11-load-more.tsx`, and the Figma Projects dataset `data-grid-projects.ts` next to
  the `data-grid/` folder.

## Date filter calendar outside the grid

For a plain `Table` filter row, `Table.FilterCell type="date"` takes `onDateSelect(date)`: the
calendar icon becomes a button (and Alt+Down in the input) that opens the DatePicker calendar in a
Popover and passes the picked ISO day. The cell does not change its own text, so set the filter
value in the callback. `calendarValue` is the ISO day to open on, `calendarLabel` names the button
(default "Choose a date for {label}"), and `calendarHeader` adds content above the days. Without
`onDateSelect` the icon is decorative.
