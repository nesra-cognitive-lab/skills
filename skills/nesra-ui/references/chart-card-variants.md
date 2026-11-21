# Chart card: chart variants

Source: Figma Lymo `Chart card` (`15788:9299`), `Chart widget/*`, `Area chart` (`15798:12557`), `Bar chart` (`15798:15774`). Docs: `components/chart-card/#chart-variants` (one complete, copyable example per variant). Storybook: `Components/Charts/Chart widgets/Chart Widgets` (Light and Dark).

## Where each Figma chart lives

The docs sidebar (group **Charts**) and Storybook (`Components/Charts/<group>/<chart>`) follow the Figma Lymo Charts page: Chart - Published Components in canvas order, then the Chart card and its parts, then Example Chart - Published Components (`Chart widget/*`). Docs overview: `components/charts`. Slugs stay under `components/`.

**1. Chart components** (Figma "Chart - Published Components")

| Figma set | Docs page | Storybook title | Component, key props |
| --- | --- | --- | --- |
| `Line chart` `15797:11273` | `components/metrics` ("Line, Area, Bar, Pie, Combo") | `Components/Charts/Chart components/Line Chart`, `…/Line Chart Plot` | `LineChart` (`dashed`, `size`, `valueDomain`), `LineChartPlot` |
| `Area chart` `15798:12557` | `components/metrics` | `…/Chart components/Area Chart` | `AreaChart` (`stacked`, `stackedPercent`, `size`, `valueDomain`) |
| `Composed chart` `15803:17145` | `components/metrics` | `…/Chart components/Combo Chart` | `ComboChart` (`type`, `valueDomain`, `lineValueDomain`) |
| `Sparkline` `15806:16542` | `components/sparkline` | `…/Chart components/Sparkline` | `Sparkline` (`trend`, `type`) |
| `Bar chart` `15798:15774` | `components/metrics` | `…/Chart components/Bar Chart` | `BarChart` (`orientation`, `stacked`, `stackedPercent`, `palette` opt-in, `highlightIndex`, `valueDomain`) |
| `Radar chart` `15833:24173` | `components/radar-chart` | `…/Chart components/Radar Chart` | `RadarChart` (`variant`, `size`) |
| `Pie chart` `15805:16615` | `components/metrics` | `…/Chart components/Pie Chart` | `PieChart` (`type`, `size`, `centerValue`, `centerLabel`, `showLabels`) |
| `Radial chart` `15835:23381` | `components/radial-chart` | `…/Chart components/Radial Chart` | `RadialChart` (`type`, `size`, `showValueTag`) |
| `Scatter chart` `15889:27419` | `components/scatter-chart` | `…/Chart components/Scatter Chart` | `ScatterChart` (`size`, `showAxisLabels`, `sizeLabel`) |
| `Scatter chart` Type=Packed bubble | `components/packed-bubble-chart` | `…/Chart components/Packed Bubble Chart` | `PackedBubbleChart` |
| `Heatmap` `15887:25863` | `components/heatmap` | `…/Chart components/Heatmap` | `Heatmap` (`type`, `size`, `getLevel`) |
| `Waterfall chart` `15891:27791` | `components/waterfall-chart` | `…/Chart components/Waterfall Chart` | `WaterfallChart` (`size`, `showStartLine`) |
| `Funnel chart` `15894:27449` | `components/funnel-chart` | `…/Chart components/Funnel Chart` | `FunnelChart` (`type`, `size`) |
| `Sankey diagram` `15899:27254` | `components/sankey-chart` | `…/Chart components/Sankey Chart` | `SankeyChart` (`size`) |

**2. Chart card and parts**

| Figma | Docs page | Storybook title | Component |
| --- | --- | --- | --- |
| `Chart card` `15788:9299`, `_Chart header`, `_Chart source`, `_Chart reference line` | `components/chart-card` | `Components/Charts/Chart card and parts/Chart Card` | `ChartCard`, `ChartReferenceLine` |
| `Chart legend` `15787:8489` | `components/chart-legend` | `…/Chart card and parts/Chart Legend` | `ChartLegend` |
| Plot bands and markers | `components/metrics#overlays` | `…/Chart card and parts/Chart Overlay` | `ChartBand`, `ChartMarker` |
| `_Chart quadrant label`, `_Chart image bubble` | `components/pattern-charts` | `…/Chart card and parts/Quadrant parts` | used by `QuadrantChart` |

**3. Chart widgets** (Figma "Example Chart - Published Components")

| Figma set | Docs page | Storybook title | Charts |
| --- | --- | --- | --- |
| Change over time, Periods, Magnitude, Part to whole, Correlation, Flow | `components/chart-card#chart-widgets`, `#chart-variants` | `Components/Charts/Chart widgets/Chart Widgets` | `ChartCard` + one chart |
| Patterns | `components/pattern-charts` | `…/Chart widgets/Proportion Bar Chart`, `Timeline Band Chart`, `Range Bar Chart`, `Lollipop Chart` | `ProportionBarChart`, `TimelineBandChart`, `RangeBarChart`, `LollipopChart` |
| Correlation · Quadrant | `components/pattern-charts` | `…/Chart widgets/Quadrant Chart` | `QuadrantChart` |

## Rules

- A Figma `Chart widget/*` is `ChartCard` + one chart. There is no `ChartWidget` export.
- A chart inside `ChartCard` drops its own surface, visible title/summary, and series legend. Still pass `title` and `summary` (screen reader name and description). Put the legend on the card: `<ChartCard legend={<ChartLegend items={...} />}>`.
- Legend marker: `square` for bar and area, `line` for line. No card legend for a single-series bar: every bar is the series `color` and the header title names it.
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
| Vertical bar | Magnitude · Vertical bar (`15959:42204`) | `BarChart`, 1 series, no `palette` (all bars `series-1`) |
| Horizontal bar | Magnitude · Horizontal bar (`15959:43300`) | `BarChart orientation="horizontal"`, 1 series, no `palette` |
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

## Single-series bar color

- Default (Figma): a single, unstacked `BarChart` series paints **every bar in the series `color`**. Figma Magnitude Vertical bar, Horizontal bar, and the base `Bar chart` Type=Single are all `series-1`, in Light and Dark. Do not pass `palette`.
- `palette="series"` or `palette="heat"` is an **opt-in** for one color per category; the chart legend then lists each category. Figma has no single-series bar with a color per category, so use it only when the user asks for it.
- `highlightIndex` keeps one bar at full color and dims the others to 35% in both modes.
- Do not fake one color with `stacked` on one series.

```tsx
const gmv = [{ key: "gmv", label: "GMV", color: "series-1" }] satisfies ChartSeries[];
// Figma default: every bar series-1
<BarChart title="Daily GMV" summary="…" data={days} series={gmv} />
// Opt in: one color per category, legend names each category
<BarChart title="GMV by channel" summary="…" data={channels} series={gmv} palette="series" />
```

## Value axis (`valueDomain`)

`LineChart`, `AreaChart`, `BarChart`, and `ComboChart` take `valueDomain: "nice" | "data" | [min, max]`.

- `"nice"` (default, Figma): the axis rounds out to round ticks, four or five steps apart, whichever ends closer to the data (95 → 100 in 25s, 66 → 80, 380K → 400K, 13K → 16K, 203 → 250). Steps are 1, 2, 2.5, 4, or 5 × a power of ten. Zero stays on the axis, negatives round down, all zeros give 0–1, stacked charts round the stacked total. Leave it unset for Figma parity.
- `"data"`: the axis ends at the data extent (with zero). This was the 1.x behavior.
- `[min, max]`: a fixed axis (for example to compare several cards on one scale, or a 0–100 score). Ticks land on both ends in round steps when possible (`[0, 150]` → every 25). Values outside are clipped; `min` must be below `max`.
- `stackedPercent` ignores it and stays 0–100%. `ComboChart` adds `lineValueDomain` for the right axis (defaults to `valueDomain` when that is `"nice"`/`"data"`, else `"nice"`). `LineChartPlot` keeps its own rounding and has no `valueDomain`.

```tsx
<BarChart title="…" summary="…" data={data} series={series} />                        // nice (default)
<BarChart title="…" summary="…" data={data} series={series} valueDomain="data" />     // follow the data
<AreaChart title="…" summary="…" data={data} series={series} valueDomain={[0, 150]} /> // fixed range
<ComboChart title="…" summary="…" data={data} barSeries={bar} lineSeries={line} lineValueDomain="data" />
```

Other charts (`WaterfallChart`, `RangeBarChart`, `QuadrantChart`, `ScatterChart`, `LollipopChart`, …) do not take `valueDomain`.

## Other Figma charts (Pie, Scatter, Heatmap, Radial, Radar, Sankey, Waterfall, Funnel)

Docs pages name each Figma node and show every supported type with copyable code; Storybook has Light and Dark stories for each, and Chart Widgets has the card version (Part to whole, Correlation, Flow, Magnitude · Radar) in Light and Dark.

| Chart | Figma | Types and key props | Visual rules |
| --- | --- | --- | --- |
| `PieChart` | `Pie chart` `15805:16615` | `type` `"pie"`, `"donut"` (default), `"nested-donut"` (`innerValue`, `ringLabels`); `size`; `centerValue`, `centerLabel`; `showLabels` (Figma Labels, off by default) | At most 5 slices, sorted largest first; they start at 12 o'clock clockwise. Diameter 136/216/296 in 198/278/358 plots. Donut hole 61%; nested rings 75–100% and 46–68.5%, both full color. Center: value on top (Semibold, by size), label under it (Text xs `text-tertiary`). `showLabels`: each share 14px outside its slice, Text xs Medium `text-secondary`, no leader line. In a card: `ChartLegend` square markers on the right. |
| `ScatterChart` | `Scatter chart` `15889:27419` | Single (one series), Multi series (≤ 3 series), Bubble (`size` per point + `sizeLabel`); chart `size` `"sm"`/`"md"` (default)/`"lg"` = 198/278/358px; `showAxisLabels`. Packed bubble → `PackedBubbleChart`. | 8px dots with a 2px `bg-primary` ring; bubbles 72% opacity, 2px ring, ~22–40px. Horizontal dashed grid only. Axis names hidden by default like Figma (always in the hidden table); `showAxisLabels` shows them. Set axis `domain` (`[0, 20_000_000]`) and short `formatValue` (`20M`). Card legend markers `dot`. |
| `Heatmap` | `Heatmap` `15887:25863`, `_Chart heat cell` `15883:23967` | `type` `"matrix"`, `"calendar"`; `size`; `getLevel` (0–4) | `heat-1`…`4` and `heat-empty`; the scale reverses in Dark (light = high), as in Figma. Values only in tooltip and table. |
| `RadialChart` | `Radial chart` `15835:23381` | `type` `"progress-ring"`, `"rings"` (1–3), `"gauge"`; `size`; `centerValue`, `caption`, `showValueTag`, `minLabel`/`maxLabel` | Gray track, round caps, dashed inner guide. Value tag: `gauge` shows the value through `formatValue` (`4,3x`), `progress-ring` the percent (`72%`), `rings` none. Round percents to whole numbers with ``formatPercent={(r) => `${Math.round(r * 100)}%`}`` to match Figma (`72%`). |
| `RadarChart` | `Radar chart` `15833:24173` | `variant` `"area"` (Single / Multi series by series count), `"dots"`; 5, 6, or 8 axes; `size` | Alternating ring bands, hollow vertex dots at the outer corners (Figma `Vertex`), 8px series dots. Figma axis names: Reach, Engagement, Conversion, AOV, Retention, Content, Reviews, Repeat buyers. |
| `SankeyChart` | `Sankey diagram` `15899:27254` | `nodes`, `links`, `size` | Matches Figma; ribbons at `chart-flow-opacity`, 80% on hover. |
| `WaterfallChart` | `Waterfall chart` `15891:27791` | `data` with `kind` `"total"`/`"change"`, `size`, `showStartLine` | Matches Figma; signed change labels. |
| `FunnelChart` | `Funnel chart` `15894:27449` | `type` `"bars"`, `"columns"` (`highlightedStageId`), `"flow"`; `size` | Matches Figma, including `48,2K` and `65,6%` with the default locale. |

```tsx
<PieChart title="GMV share by channel" summary="…" data={top5} size="md" centerValue="Rp 84M" centerLabel="Total GMV" showLabels />
<ScatterChart title="Ad spend vs GMV" summary="…" series={[tiktok]} data={campaigns} size="md" /* showAxisLabels */
  xAxis={{ label: "Ad spend", domain: [0, 20_000_000], formatValue: (v) => `${v / 1e6}M` }}
  yAxis={{ label: "GMV", domain: [0, 80_000_000], formatValue: (v) => `${v / 1e6}M` }} />
<RadialChart title="ROAS" summary="…" type="gauge" max={6} caption="of 6x target" minLabel="0x" maxLabel="6x"
  showValueTag formatPercent={(r) => `${Math.round(r * 100)}%`} data={[{ id: "roas", label: "ROAS", value: 4.3 }]} />
```

Number format: every chart formats numbers in the `NesraI18nProvider` locale, `id-ID` without a provider, as in Figma: values `1.250.000`, percents `65,6%`, compact plot labels `48,2K` (K, M, B, T). Do not add `id-ID` formatters for that; pass a format prop only to add a unit or currency (`` formatValue={(v) => `Rp ${v.toLocaleString("id-ID")}`} ``). For English numbers: `<NesraI18nProvider locale="en-US">`. `useNesraLocale()` (from `@nesra/ui/i18n`) returns the locale. Dates keep their own `locale` props.

## Not supported yet (do not promise; waiting on a decision)

- Figma `_Chart bar` State=Muted is `bg-quaternary` (gray), and Periods Bar highlight shows gray bars with one `series-1` bar. `highlightIndex` dims the other bars to 35% of their own color instead. Figma Hover does not dim the other bars; the library dims them to 35% on hover.
- Figma `_Chart bar` State=Active shows a value tag above the bar (`Rp 84M`); the library has no value tag on bars.
