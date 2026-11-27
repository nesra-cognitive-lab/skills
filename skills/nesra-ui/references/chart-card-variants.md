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
| Correlation · Quadrant (Product Potential Map) | `components/pattern-charts#product-potential-map` | `…/Chart widgets/Quadrant Chart` › `Figma`, `Figma · Hover`, `Figma · Dark`, `Figma · Hover · Dark` (same card: Chart Widgets › `Correlation · Quadrant`) | `ChartCard` + `QuadrantChart` |

## Rules

- A Figma `Chart widget/*` is `ChartCard` + one chart. There is no `ChartWidget` export.
- A chart inside `ChartCard` drops its own surface, visible title/summary, and series legend. Still pass `title` and `summary` (screen reader name and description). Put the legend on the card: `<ChartCard legend={<ChartLegend items={...} />}>`.
- Legend marker: `square` for bar and area, `line` for line. No card legend for a single-series bar: every bar is the series `color` and the header title names it.
- Series colors: `series-1` … `series-8`, in series order. Same `key`/`label`/`color` in the chart `series` and in `ChartLegend` `items`.
- Values inside the plot stay short (`formatValue={(v) => \`Rp ${v.toLocaleString("id-ID")}M\`}` on data in millions). The full amount goes in `ChartCard.Header` `value` (`Rp 146.000.000`).
- Header as in Figma widgets: `icon` (`ChartLine` for line/area, `ChartColumn` for bar), `title`, `info`, `subtitle`, `tabs` (the period control: on the Nesra Scale trend cards, A4 `7944:36722` and P11 `8205:67920`, a `ButtonGroup size="xs"` with Day/Week/Month/Year; see [Header layout](#header-layout-the-controls-wrap-under-the-title-chartcardheader)), `actions` (the 62 x 28px `SegmentedControl` Bar/Line chart type switch on those trend cards, or a UtilityButton tertiary `EllipsisVertical` as in Lymo), `value`, `change` (Badge `pill-color` success + `ArrowUp`), `comparison`. Then `ChartCard.Source sources={[...]} updated="..."`.
- Source row alignment (since design-system-nesra PR #330): leave `ChartCard.Source` at the default `align="start"` (Figma Lymo `_Chart source`: icon, label, and sources left, updated time right) unless the Figma frame centers it. **Nesra Scale screens center it**: the Overview, Product, and Shop trend cards (Overview A1 `7910:103408`, Product A1 `7702:34006`, Shop A1 `7914:70`) use `align="center"`, without `updated`. With `updated`, a centered Source puts the time on its own centered row (interim decision; Figma has no example). Check the frame: the label text (`Source` or `Source :`) is a prop and differs between frames. Do not center the row with `className` (`justify-center` on the footer) or by wrapping it.

## Header layout: the controls wrap under the title (`ChartCard.Header`)

Source: Figma Lymo `_Chart header` (`15769:7276`), Top row (`15769:7277`). Spacing fixed in design-system-nesra PR #351. Docs: `components/chart-card/#header-layout`. Storybook: `Components/Charts/Chart card and parts/Chart Card` › `Header Short Subtitle`, `Header Long Subtitle`, `Header Side By Side`, `Header At Figma Width` (Light and Dark).

The top row of `ChartCard.Header` is a wrapping row. The **title group** (`icon`, `title`, `info`, `subtitle`) is at the start and the **controls** (`tabs`, then `actions`, as one group) are at the end.

- **They share the title row only while they fit.** Then the controls sit at the right edge, top aligned with the title group. The title group ends with 16px of padding and the row has no column gap, so the space between the text and the controls is 16px or more.
- **When they do not fit, the controls move under the title group.** The whole controls group starts a new line, 12px below the title group, at the left edge. This is the Lymo behavior ("on a narrow card the tabs and actions move under the title"), not a bug. There is no prop to keep them on the title row.
- **They fit when** the title group width + 16px + the controls width is at most the header width. The header is the card width minus 42px (20px padding and a 1px border on each side).
- **Control sizes (Nesra Scale `Chart controls`, `7772:71517`, as A4 and P11 draw them): 271 x 28px in Figma, about 268px in the DS.** Use `ButtonGroup size="xs"` for the period (Lymo Button group Size Tiny: 26px items in a 28px container, Text xs Medium, 10px item padding; Day, Week, Month, and Year are about 198px in the DS, 201px in Figma) + 8px + the `SegmentedControl` Bar/Line switch (62 x 28px: 2px padding, two 28 x 24px segments, 14px icons) = about 268px. With a 299px subtitle (title group 315px) the header must be about 583px wide (Figma P11 is 591.5px), a card of about 625px. A 600px card puts the controls under the title.
```tsx
<ChartCard.Header
  title="Actualization trend"
  subtitle="Nov 1 - 13, 2026 vs Oct 1 - 13, 2026"
  tabs={
    <ButtonGroup aria-label="Period" size="xs" value={[period]} onValueChange={([next]) => next && setPeriod(next)}>
      <ButtonGroup.Item value="day">Day</ButtonGroup.Item>
      <ButtonGroup.Item value="week">Week</ButtonGroup.Item>
      <ButtonGroup.Item value="month">Month</ButtonGroup.Item>
      <ButtonGroup.Item value="year">Year</ButtonGroup.Item>
    </ButtonGroup>
  }
  actions={
    <SegmentedControl aria-label="Chart type" value={type} onValueChange={setType}>
      <SegmentedControl.Item aria-label="Bar chart" icon={<ChartColumn />} value="bar" />
      <SegmentedControl.Item aria-label="Line chart" icon={<ChartLine />} value="line" />
    </SegmentedControl>
  }
/>
```
- **Do not use `Tabs` for the trend card period.** `Tabs variant="minimal" size="sm"` (Lymo Horizontal tabs Button minimal, `15281:2148`) is 38px tall and about 242px wide for the same four labels, so the controls come to about 312px and the same subtitle needs a header of about 627px (a card of about 669px). The `Tabs` list matches Lymo; A4 and P11 simply do not draw it (Nesra Scale swaps the `_Chart header` Period tabs for the Button group above). The hidden Period tabs instance inside Lymo `_Chart header` points at an old "Button border" tab (253 x 44px) that is no longer in the Lymo `Horizontal tabs` set; ignore it.
- **Known difference, not decided yet:** the Nesra Scale trend card (P11 `8130:128201`) is 617.5px wide with a 591.5px header, so its inset is 13px a side (12px padding + 1px border), whereas `ChartCard` is Lymo's 20px padding + 1px border (21px a side). In a 617.5px `ChartCard` the header is 575.5px, so a 299px subtitle plus the controls above (about 583px) still wraps by about 8px, where Figma shows one row. Do not override the card padding in the app; ask for a DS decision.
- **Nothing is truncated.** Do not add `truncate`, `line-clamp`, `max-w-*`, or `whitespace-nowrap` to the title or subtitle, and do not set widths on the header parts. A subtitle longer than the row wraps inside the title group. The controls keep their width and never shrink: controls wider than the header extend past its end edge (about 268px of controls in a 300px card), so choose controls that fit your narrowest card.
- **Two cards side by side line up only when both headers take the same number of lines.** A card whose subtitle fits beside the controls has a one-line header, and a card whose subtitle does not has a two-line header, so its plot starts lower by at least the controls' height plus 12px (50px with the controls above). Figma does not align them and neither does `ChartCard`. For a Comparison view, use subtitles of the same length (the same date format in both cards) and card widths where both fit, not a width near the point where the controls wrap. Never fix the difference with a spacer, a fixed header height, or a `min-h` on the header; check the layout at the widths you ship instead.

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

## Series toggle (hide a series from the legend)

Since design-system-nesra PR #327 (not in npm 1.0.0; use the 2.x tarball or a later 2.x release). Nesra Scale Overview A-S3 (`8109:94915`): clicking a legend item hides its series. Figma Lymo draws a hidden item as `_Chart legend item` State=Muted.

- The state is **controlled and lives in your component**. Pass the same `string[]` of hidden keys to `ChartLegend hiddenSeries` and to the chart's `hiddenSeries` (`BarChart`, `AreaChart`, `ComboChart`). Pass `onHiddenSeriesChange` to the legend: every item then becomes a `<button aria-pressed>` (true while the series shows), and click, Enter, or Space calls it with the new list.
- A hidden series draws no bar, segment, area, or line, and has no tooltip row. **The axes keep the range of every series**, including the right axis of a hidden line or second bar, so nothing moves. The hidden data table keeps every series. Unknown keys are ignored, so one list can serve a current and a comparison chart.
- A hidden item looks like `muted` (marker 40%, text `text-quaternary`). Figma draws no hover state: the toggle shows only the pointer and the focus ring. Do not add hover classes.
- The DS does not stop the last series from being hidden (every series hidden shows axes only and no tooltip). If the screen needs at least one series, guard it in `onHiddenSeriesChange`.
- A chart outside a `ChartCard` draws its own legend: pass `hiddenSeries` and `onHiddenSeriesChange` to the chart itself. A `BarChart` with `palette` names categories, so its legend never toggles.
- `LineChart` has no `hiddenSeries`.

```tsx
const [hidden, setHidden] = useState<string[]>([]);
<ChartCard
  legend={
    <ChartLegend
      items={[
        { key: "gmv", label: "Total GMV", color: "series-1" },
        { key: "cost", label: "Total Cost", color: "series-2" },
      ]}
      hiddenSeries={hidden}
      onHiddenSeriesChange={setHidden}
    />
  }
>
  <ChartCard.Header title="Actualization trend" subtitle="Trend data for Nov 1 - 13, 2026" />
  <ComboChart type="bar-bar" barSeries={gmv} lineSeries={cost} hiddenSeries={hidden} … />
</ChartCard>
```

Do not filter `series`, `barSeries`, or `data` yourself to hide a series: that rescales the axes, removes the series from the data table, and breaks `ComboChart`, which needs both `barSeries` and `lineSeries`.

## Value tag on the active category

Since design-system-nesra PR #328. `BarChart` and `ComboChart` take `activeValueTag={(point) => ReactNode}`: the Figma Lymo `_Chart value tag` (`15727:5191`) on the active category, as Nesra Scale Overview A4 (`7944:36722`) shows the hovered day's Total GMV.

- **Active category:** the hovered one, the one explored with the arrow keys, or the one hovered on a chart with the same `syncId` (current and comparison charts tag the same index together). Without hover, a `BarChart` shows it on `highlightIndex` (Figma `_Chart series/Bar` State=Highlight). Escape hides it with the tooltip.
- **Placement:** one tag per category, centered on it, 6px above the category's tallest bar (a stack is one bar). Horizontal `BarChart`: 8px past the longest bar, centered on the row. `ComboChart` measures the bars of `bar-line`, `stacked-bar-line`, `bar-area`, and `bar-bar` on their own axes; lines, areas, and series hidden with `hiddenSeries` (design-system-nesra #327) are not counted, and with every bar series hidden there is no tag; `area-line` and `area-area` draw no tag.
- **Text:** whatever the callback returns, such as `rupiah(point.values.gmv)` (A4 writes the full `Rp 209.023.400`). Return `undefined` for no tag on a point (for example a `null` value). The pill is styled by the DS: 24px, `bg-primary`, 1px `border-secondary`, `shadow-overlay-md`, Text xs Semibold `text-primary`. Do not wrap the text in your own pill or `Badge`.
- The tag is `aria-hidden`; the tooltip and the hidden data table carry the value.
- Not the same as `BarChart valueLabel`, which is always-visible SVG text after every bar (What moved).

```tsx
<ComboChart
  type="bar-bar"
  barSeries={{ key: "gmv", label: "Total GMV", color: "series-1" }}
  lineSeries={{ key: "cost", label: "Total Cost", color: "series-2" }}
  syncId="overview-trend"
  activeValueTag={(point) => (point.values.gmv == null ? undefined : rupiah(point.values.gmv))}
  …
/>
```

## Tooltip rows without a series (`tooltipExtraRows`)

Since design-system-nesra PR #347. `AreaChart`, `BarChart`, and `ComboChart` take `tooltipExtraRows={(point) => rows}`. It draws a plain row in the hover tooltip that is **not a series**: a label and a value with no color marker. Nesra Scale TikTok Ads D1 `8525:335933` (GMV Max Live trend, hover Oct 10) is the reference:

| Row | Marker | Comes from |
| --- | --- | --- |
| `Cost per Live View  Rp 729` | series 1 | the series |
| `Total Spend  Rp 18.000.000` | series 2 | the series |
| `Gross Revenue  Rp 147.330.195` | **none** | `tooltipExtraRows` |
| divider, then `Total ROI  8,19x` | none | `tooltipTotal` (already existed) |

- **Figma facts.** The Figma Lymo `Chart tooltip` (`15393:2668`, Rows 1 to 6) has a marker on every row and no marker-less row. D1 draws the row as a Rows row with the marker removed. So the option is opt-in; a chart without it draws the Lymo tooltip. D-T1 to D-T13 (lane `8525:220612`) draw only the `10.10` hover pill and no tooltip, so Figma shows `Gross Revenue` for D1 only (Day, Oct 1 - 31). Do not assume it shows or hides for Week, Month, or the comparison charts; follow the product decision.
- **Shape.** `(point: ChartPoint) => readonly { key?: string; label: ReactNode; value: ReactNode }[] | undefined`. The type is `ChartTooltipExtraRow`, exported from `@nesra/ui` and from `@nesra/ui/area-chart`, `@nesra/ui/bar-chart`, and `@nesra/ui/combo-chart`. Return `undefined` or `[]` for a day with no extra row.
- **Placement and style (do not restyle).** The rows come after the series rows and before the `tooltipTotal` divider, in the order you return them. The label starts where a series marker starts (not after a 12px marker and 6px gap), in Text xs Regular `text-tertiary`. The value is Text xs Semibold `text-primary`, right-aligned in the same column as the series values.
- **Values are yours to format.** `label` and `value` are `ReactNode` and are not formatted by the chart: pass the final text, as for `tooltipValue` and `tooltipTotal`. Write full Rupiah (`Rp 147.330.195`), and format with the app locale (`useNesraLocale` / `NesraI18nProvider`, id-ID by default) rather than hard-coding separators. A `0` stays `0`, so pass `Rp 0` for a zero. A `null` or `undefined` value shows "No value", as a series row does. Pass `null` for a day without data; never `"-"` or `"0"`.
- **Tooltip only.** The rows are not in the hidden data table. A value readers need without hovering belongs in the summary or a table beside the chart.
- **Long labels** widen the tooltip up to the chart width, then wrap; keep labels short.
- **Do not** fake the row with a hidden or transparent series (it changes the axes, the legend, and the data table), with a long string in `tooltipValue`, with `tooltipTotal` (that one draws the divider and the Medium label), or with a custom tooltip. Derived figures such as `Total ROI` stay in `tooltipTotal`.
- Not available on `LineChart` or the other charts yet; their tooltips are unchanged.

```tsx
<ComboChart
  type="bar-bar"
  data={days}
  barSeries={{ key: "cplv", label: "Cost per Live View", color: "series-1" }}
  lineSeries={{ key: "spend", label: "Total Spend", color: "series-2" }}
  tooltipExtraRows={(point) => [
    { label: "Gross Revenue", value: rupiah(grossRevenue[point.id]) }, // null shows "No value"
  ]}
  tooltipTotal={(point) => ({ label: "Total ROI", value: `${roi(point)}x` })}
/>
```

## Empty category note

Since design-system-nesra PR #328. `ChartCategoryNote` from `@nesra/ui/chart-overlay` (Nesra Scale Overview P6, `8127:129962`) puts a `Badge` sm Gray, such as "No 2025 data yet", centered on a category, 8px above the baseline (horizontal `BarChart`: 8px right of the baseline, centered on the row). Place it as a chart child like the other overlays; its `label` is listed with the chart data.

- It does not read the values. Give that category `null` for every series (missing, never `0`) and pass the note only for it. An unknown `at` throws.
- Tested in `BarChart` and `ComboChart`, the charts Overview uses.

```tsx
<ComboChart type="bar-bar" data={[{ id: "2025", label: "2025", values: { gmv: null, cost: null } }, …]} …>
  <ChartCategoryNote at="2025" label="No 2025 data yet" />
</ComboChart>
```

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

## Product Potential Map (Quadrant)

Figma `Chart widget/Correlation`, Chart=Quadrant (Default `16851:45965`, Hover `16855:46210`, Dark preview). Docs: Pattern Charts › Quadrant › Product Potential Map (full code, anatomy). Storybook: Quadrant Chart › `Figma` (+ `· Hover`, `· Dark`, `· Hover · Dark`) at the Figma size, 1099px card and 724px plot; `Default` and `RangeSliders` are the plot only.

```tsx
<ChartCard>
  <ChartCard.Header title="Product Potential Map" info="…" subtitle="Top 20 products by GMV. Product clicks vs CTOR, bubble size = GMV, dashed lines are the average"
    actions={<>
      <DropdownMenu>
        <DropdownMenu.Trigger render={<Button variant="outline" size="xs" />}><Globe aria-hidden="true" />{channel}<ChevronDown aria-hidden="true" /></DropdownMenu.Trigger>
        <DropdownMenu.Content align="end"><DropdownMenu.RadioGroup value={channel} onValueChange={setChannel}>{/* RadioItems */}</DropdownMenu.RadioGroup></DropdownMenu.Content>
      </DropdownMenu>
      <Switch size="sm" label="Show movement" checked={on} onCheckedChange={setOn} />
    </>} />
  <QuadrantChart title="Product Potential Map" summary="…" data={top20 /* { id, label, x, y, size, image } */} sizeLabel="GMV"
    formatSize={(v) => `Rp ${v.toLocaleString("id-ID")}`}
    xAxis={{ label: "Product clicks", domain: [0, 100_000], average: 36_111, averageLabel: "Avg clicks 36.111",
      formatTick: (v) => (v === 0 ? "0" : `${(v / 1000).toLocaleString("id-ID")}K`) }}
    yAxis={{ label: "CTOR", domain: [0, 4], average: 1.52, formatValue: (v) => `${v.toLocaleString("id-ID")}%` }}
    quadrants={{ topLeft: { name: "Potential", description: "Low clicks · High CTOR", color: "blue" },
      topRight: { name: "Winning", description: "High clicks · High CTOR", color: "success" },
      bottomLeft: { name: "Need attention", description: "Low clicks · Low CTOR", color: "error" },
      bottomRight: { name: "Emerging", description: "High clicks · Low CTOR", color: "warning" } }}
    rangeSliders />
</ChartCard>
```

Rules:
- The channel control is a Figma `Button` Outline Tiny (26px), not `Select` (smallest Select is 34px). The toggle is `Switch size="sm"` with `label`.
- Pass the zone `description`s with short axis names (`Low clicks · High CTOR`); the default uses the full axis label (`Low Product clicks · …`).
- The dashed lines are catalog averages in Figma (`36.111`, `1,52%`), not the mean of the 20 points: pass `average`. The X tag needs `averageLabel` (`Avg clicks …`, shorter than the axis label); the Y tag default `Avg CTOR 1,52%` already matches.
- Ticks vs values: `formatTick` writes the axis ticks only (`25K`); `formatValue` writes the tooltip, the average tag, and the hidden table, and the ticks when `formatTick` is not set. Leave X `formatValue` empty so the tooltip uses the locale default (`51.844`); give Y a `formatValue` only for the `%` unit.
- Bubbles: `image` per point (product photo; use your own images), `size` = GMV (24–56px by area), `label` = full product name. Names show under the bubble, 104px wide, two lines, cut with `…` automatically.
- Bubble names have the Figma outline built in: a 3px stroke outside the letters in `bg-primary` (white in Light, `#0A0A0A` in Dark), no shadow or background box. Do not add a `text-shadow` or a background behind names.
- Built-in layout per Figma: plot 724px tall, legend over the Y ticks, right zone labels 16px from the edge, tooltip 12px from the bubble with a Dot marker, sliders named `{axis} minimum` / `{axis} maximum`.
- The Figma card is 1099px wide; in narrow containers the plot squeezes and labels overlap. Give it the full width (the docs preview scrolls sideways).

Not supported yet (waiting on a decision; do not build ad hoc):
- **Show movement**: Figma has the toggle (pressed) but draws no movement in any state. `QuadrantChart` has no movement layer; the switch is state only.
- **Static Hover state**: no prop to show a hovered bubble without a pointer (Storybook `· Hover` hovers it in a `play`).

## Not supported yet (do not promise; waiting on a decision)

- Figma `_Chart bar` State=Muted is `bg-quaternary` (gray), and Periods Bar highlight shows gray bars with one `series-1` bar. `highlightIndex` dims the other bars to 35% of their own color instead. Figma Hover does not dim the other bars; the library dims them to 35% on hover.
- Figma `_Chart bar` State=Muted's gray bars: see above. The `_Chart value tag` of State=Active is supported since design-system-nesra #328: see [Value tag on the active category](#value-tag-on-the-active-category).
