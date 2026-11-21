# Chart card: chart variants

Source: Figma Lymo `Chart card` (`15788:9299`), `Chart widget/*`, `Area chart` (`15798:12557`), `Bar chart` (`15798:15774`). Docs: `components/chart-card/#chart-variants` (one complete, copyable example per variant). Storybook: `Components/Charts/Chart Widgets` (Light and Dark).

## Rules

- A Figma `Chart widget/*` is `ChartCard` + one chart. There is no `ChartWidget` export.
- A chart inside `ChartCard` drops its own surface, visible title/summary, and series legend. Still pass `title` and `summary` (screen reader name and description). Put the legend on the card: `<ChartCard legend={<ChartLegend items={...} />}>`.
- Legend marker: `square` for bar and area, `line` for line. No legend for a single-series bar (each category has its own color).
- Series colors: `series-1` … `series-8`, in series order. Same `key`/`label`/`color` in the chart `series` and in `ChartLegend` `items`.
- Values inside the plot stay short (`formatValue={(v) => \`Rp ${v.toLocaleString("id-ID")}M\`}` on data in millions). The full amount goes in `ChartCard.Header` `value` (`Rp 146.000.000`).
- Header as in Figma widgets: `icon` (`ChartLine` for line/area, `ChartColumn` for bar), `title`, `info`, `subtitle`, `tabs` (Tabs `minimal` `sm`: Day/Week/Month/Year), `actions` (UtilityButton tertiary `EllipsisVertical`), `value`, `change` (Badge `pill-color` success + `ArrowUp`), `comparison`. Then `ChartCard.Source sources={[...]} updated="..."`.

## Supported variants

| Variant | Figma | Component + key props |
| --- | --- | --- |
| Line | Change over time · Line (`15958:41546`) | `LineChart`, ≥1 series, marker `line` |
| Area (multi area) | Change over time · Area (`16350:45608`), Periods · Multi area | `AreaChart`, ≥1 series, no `stacked` |
| Stacked area | Area chart Type=Stacked (`15798:11626`) | `AreaChart stacked` |
| Stacked 100% area | Area chart Type=Stacked 100% (`15798:12183`) | `AreaChart stackedPercent` |
| Vertical bar | Magnitude · Vertical bar (`15959:42204`) | `BarChart`, 1 series |
| Horizontal bar | Magnitude · Horizontal bar (`15959:43300`) | `BarChart orientation="horizontal"`, 1 series |
| Grouped bar | Magnitude · Vertical grouped bar (`15959:42737`) | `BarChart`, ≥2 series, no `stacked` |
| Horizontal grouped bar | Magnitude · Horizontal grouped bar (`15959:43735`) | `BarChart orientation="horizontal"`, ≥2 series |
| Stacked bar | Periods · Stacked bar (`15960:57533`) | `BarChart stacked` (values ≥ 0) |
| Stacked horizontal bar | Bar chart Horizontal Stacked (`15798:15162`) | `BarChart orientation="horizontal" stacked` |
| Stacked 100% bar | Bar chart Vertical Stacked 100% (`15798:14056`) | `BarChart stackedPercent` |
| Stacked 100% horizontal bar | Bar chart Horizontal Stacked 100% (`15798:15518`) | `BarChart orientation="horizontal" stackedPercent` |
| Bar with average line | Periods · Bar highlight | `BarChart highlightIndex={n}` + `<ChartReferenceLine value label />` child |
| Bar and line / combos | Change over time · Bar and line | `ComboChart type="bar-line" \| "stacked-bar-line" \| "area-line" \| "bar-area"` |

`stackedPercent` draws a 0–100% axis; the tooltip and hidden table keep the real values. Bar chart Type=Stacked 100% **is** supported now (older notes said it was not).

## Short examples (inside a `ChartCard` with header and source)

```tsx
const series = [
  { key: "tiktok", label: "TikTok Shop", color: "series-1" },
  { key: "shopee", label: "Shopee", color: "series-2" },
  { key: "tokopedia", label: "Tokopedia", color: "series-3" },
] satisfies ChartSeries[];
const legend = <ChartLegend items={series.map((s) => ({ ...s, marker: "square" as const }))} />;

// Area
<ChartCard legend={legend}>…<AreaChart title="GMV by channel" summary="…" data={data} series={series} />…</ChartCard>
// Stacked area / Stacked 100% area
<AreaChart title="…" summary="…" data={data} series={series} stacked />
<AreaChart title="…" summary="…" data={data} series={series} stackedPercent />
// Horizontal bar, one series: no card legend
<BarChart title="…" summary="…" data={channels} series={[{ key: "gmv", label: "GMV", color: "series-1" }]} orientation="horizontal" />
// Grouped bar, vertical / horizontal
<BarChart title="…" summary="…" data={data} series={series} />
<BarChart title="…" summary="…" data={data} series={series} orientation="horizontal" />
// Stacked bar, vertical / horizontal / 100%
<BarChart title="…" summary="…" data={data} series={series} stacked />
<BarChart title="…" summary="…" data={data} series={series} orientation="horizontal" stacked />
<BarChart title="…" summary="…" data={data} series={series} stackedPercent />
<BarChart title="…" summary="…" data={data} series={series} orientation="horizontal" stackedPercent />
// Line: legend marker "line"
<ChartCard legend={<ChartLegend items={series.map((s) => ({ ...s, marker: "line" as const }))} />}>…<LineChart … />…</ChartCard>
```

For the full card code, copy from the docs example of the same name.

## Not supported yet (do not promise; waiting on a decision)

- Single-series bar in one color. Figma `Vertical bar` and `Horizontal bar` (Magnitude) paint every bar `series-1`. `BarChart` with one series always colors each category from `palette` (`series` or `heat`). Do not fake it with `stacked` on one series.
- Rounded axis maximum. Figma's value axis ends at a round number (`Rp 100M`); the library's top tick follows the data maximum (`Rp 95M`).
