# Lymo components in 2.x

Use this reference in a 2.x project when a screen needs a Figma Lymo component that is not in the main decisions table. Every name below is a public export of `@nesra/ui` 2.x. Confirm the props in the installed package and the component's docs page before writing them; this file names the choice, not the full API.

Import from the subpath named in the docs page when one exists, such as `@nesra/ui/sparkline`. Heavy engines live only in their own subpath: Shiki loads lazily inside `@nesra/ui/code-snippet`, and Tiptap ships only in `@nesra/ui/text-editor-tiptap`. An app that does not import those subpaths does not bundle them.

## Base components

| Need | Use |
| --- | --- |
| An icon in a tinted frame (messages, empty states, steps, menus) | `FeaturedIcon`: `size` `tiny`–`xl`, `type` `light`, `gradient`, `dark`, `modern`, `modern-neue`. `modern` and `modern-neue` take only `color="gray"`. Do not hand-build icon frames. |
| A status dot | `Dot` with `size` `sm`, `md`, `lg`, `outline`, and a status `color` such as `success` |
| A file type icon | `FileTypeIcon` with any Figma `type` (see `FILE_TYPE_ICON_TYPES`) and `variant` `default`, `gray`, or `solid` |
| A check or rating star | `CheckIcon` (`type` `default`, `line`, `filled`; `color` `brand`, `gray`, `success`) and `StarIcon` (`value` 0–1) from `@nesra/ui/misc-icon` |
| A help icon with a tooltip next to a label | `HelpIcon` from `@nesra/ui/help-icon`, or `help` on `Field`, `Input`, `Textarea`, `Select` |
| An optional field label | `optional` on `Field`, `Input`, `Textarea`, `Select` |
| Progress as a bar or ring | `ProgressBar` (`label` `right`, `bottom`, `top-floating`, `bottom-floating`, `none`) or `ProgressCircle` (`size` `xxs`–`lg`, `shape` `circle` or `half-circle`) |
| A compact toggle | `Switch variant="slim"` |
| A segmented control item with a status dot | `ButtonGroup.Item dot` |
| A row of badges with a lead label | `BadgeGroup` |
| A 24 or 28px toolbar icon action | `UtilityButton` |
| Avatar with a count or avatar badge, a name and email, or a large profile photo | `Avatar` `status="count"` or `"avatar"`, `Avatar.LabelGroup`, `Avatar.ProfilePhoto` (`verified`) |
| A range or single value slider | `Slider` (`orientation="vertical"` for a Y range) |
| A selectable card list | `RadioGroup` `variant` (`radio-button`, `checkbox`, `icon-simple`, `avatar`, `payment-icon`, `icon-card`) |
| A loading spinner or dots | `LoadingIndicator` (`line-simple`, `line-spinner`, `dot-circle`) |
| A section or content divider | `Divider` (`container` for the Figma Section divider) |
| Code with copy and syntax colors | `CodeSnippet` (`language`, `CODE_SNIPPET_LANGUAGES`) |
| A video | `VideoPlayer` |
| A rich text field | `TextEditorEditor` from `@nesra/ui/text-editor-tiptap`; `TextEditor` is the frame and toolbar without an engine |
| Brand, payment, integration, and store marks | `SocialIcon`, `PaymentMethodIcon`, `IntegrationIcon`, `SocialButton`, `SocialButtonGroup`, `MobileAppStoreBadge` |
| Decorative backgrounds and illustrations | `BackgroundPattern`, `BackgroundPatternDecorative`, `BackgroundOverlay`, `SuperGraphicLiftedLines`, `SuperGraphicPixelInsight`, `SuperGraphicFacet`, `Illustration`, `LymoMascot`, `LymoLoader`, `LymoProgressBar`, `LymoLoadingStatus`, `EmptyState.Illustration` |

## Application components

| Need | Use |
| --- | --- |
| Switch between products in the sidebar | `Sidebar.PlatformSwitcher` with `Sidebar.PlatformMenu` and `Sidebar.PlatformLogo` |
| A product rail or a mobile bottom bar | `AppRail`, `MobileTabBar` |
| A card at the bottom of the sidebar (usage, upgrade, onboarding, support) | `Sidebar.FeaturedCard` with `Sidebar.FeaturedCardTitle`, `Sidebar.FeaturedCardDescription`, `Sidebar.FeaturedCardActions`. Compose the Figma types from other components. |
| A top header with links | Compose `NavigationMenuItem`, an icon button, and `DropdownMenu.AccountTrigger` (Page Headers docs). There is no header export. |
| A KPI group with a title, an AI insight, and a source | `KpiGroup.Header`, `KpiGroup.Insight`, `KpiGroup.Source` |
| Rich table cells, toolbar, filter row, summary, footer | `Table.*Cell` parts with `Table.LeadAction`, `Table.StateIcon`, `Table.PriorityIcon`, `Table.Toolbar`, `Table.ToolbarButton`, `Table.FilterRow`, `Table.FilterCell`, `Table.SummaryCell`, `Table.FooterBar`, `Table.StatusBar` |
| Table toolbar menus (Filter, Columns, Display, Export) | `Table.ToolbarButton` as the `render` of `DropdownMenu.Trigger` or `Popover.Trigger`; it is Active while its menu is open. Filter toggles `Table.FilterRow open`. Columns uses `DropdownMenu.CheckboxItem indicator="checkbox"` with `DropdownMenu.Search` and `DropdownMenu.FooterButton`; Display uses `DropdownMenu.RadioGroup` with `DropdownMenu.RadioItem`; Export uses `DropdownMenu.Item` with `DropdownMenu.Shortcut` (".csv") |
| A multi-value filter in the filter row (Figma Select filter, "3 of 4") | `Table.FilterCell type="multi-select"` |
| An editable spreadsheet grid (Figma Table with toolbar, filter row, totals, status bar) | The Data grid example, copied into the app. It is not an export: see [Data grid in 2.x](data-grid.md) |
| Pages, dots, or a carousel | `Pagination` (`type`, `breakpoint`), `Pagination.DotGroup`, `Carousel` |
| A tree list | `TreeView` (not `TableTree`, which is a table) |
| Account, search, or integrations in a dropdown | `DropdownMenu.AccountTrigger`, `DropdownMenu.AccountItem`, `DropdownMenu.Search`, `DropdownMenu.Footer` |
| One choice, or checkboxes, in a dropdown | `DropdownMenu.RadioGroup` with `DropdownMenu.RadioItem`; `DropdownMenu.CheckboxItem` with `indicator="checkbox"` and `indeterminate` for a Select all |
| Notification types | `Toast` `type` `avatar`, `image`, `progress`, `ai` |
| Page header styles | `PageHeader variant` (9 Figma styles) with `PageHeader.Banner`, `.Avatar`, `.Search`, `.Count` |
| A date and time field | `DateTimeInput` |
| A date with a Today button, or a date with time slots | `DatePicker` (single) and `availableTimes` with `onTimeChange` |
| Date Picker text format | Shown as `DD/MM/YYYY` (`21/11/2026`) for every `locale`: single input, placeholder, Selected date box, and range inputs. `locale` only names months and weekdays. Values stay ISO `YYYY-MM-DD`: `value`, `defaultValue`, `onValueChange`, `minDate`, `maxDate`, `isDateDisabled`, range `{ start, end }`, and form fields `name`, `name[start]`, `name[end]`. Never pass `DD/MM/YYYY` to a prop. DS exports no formatter, so format the value yourself when you show it on a page. The single input accepts typed `DD/MM/YYYY` (one-digit day and month are fine, `/` is inserted for you, a pasted ISO date works). Range inputs order day, month, year. Tests that type ISO into the input must type `DD/MM/YYYY` instead. |
| Date Picker width | The single field needs no width wrapper: it is 328px, or 528px with `availableTimes`, and narrows in a smaller container (below 528px the times become a Select). A bare calendar in a `Popover` needs only the Popover's own padding, so do not force a width on the Popover. |
| Date Picker calendar cells | Days are 40px with 4px row gaps (six weeks are 304px, single menu 328×496), so leave about 40px more height for an inline or popover calendar than in 1.x. This also applies to the Calendar side panel and the Table or Data grid date filter. Today is a `bg-secondary` fill with a brand dot, not an outline. |
| A dot under a date | `isDateMarked={(iso) => boolean}` (ISO `YYYY-MM-DD`, like `isDateDisabled`). Always set `markedDateLabel` to what the dot means (default `Marked`, for example `Has events`) so screen readers get it. Never use a dot without a label. |
| Range with a time | `showTime` with `startTime` and `endTime` (`"HH:mm"` 24-hour, or `null`) and `onTimeChange({ start, end })` on Apply. `value` and `onValueChange` stay `{ start, end }` ISO dates only; combine date and time in the app when you need an instant (use `timeZone`). Forms add `name[startTime]` and `name[endTime]` only with `showTime`. Same-day end before start is rejected. Date and time are typed together in the input; picking a day keeps the time. Below 700px the two fields stack. New type `RangeTime`; `DateTimeInput` is unchanged. |
| Range presets | `DatePicker mode="range"` presets are the nine Figma presets. All time reports an empty `start` (or `minDate`). |
| Steps with icons or numbers | `ProgressSteps` (`type` `icon`, `number`, `featured-icon`) |
| Section headings and footers | `SectionHeader`, `SectionLabel`, `SectionFooter` |
| An inline call to action | `InlineCta` |
| A command palette | `CommandMenu` |
| A color picker | `ColorPicker` with `ColorPicker.Gradient` and `ColorPicker.ImageFill`; `toCssGradient` turns a gradient value into CSS |
| An activity feed | `Timeline` with `Timeline.List divider` (`connector`, `line`, `none`) and `Timeline.Item size` (`sm`, `md`) |
| A file upload row | `FileUploadCard` `type` (`progress-bar`, `progress-fill`) and `iconType` (`file-type`, `simple`) |
| A message composer | `MessageComposer variant` `minimal`, `textarea`, or `advanced` (with `MessageComposer.Footer` and `MessageComposer.FooterButton`) |
| A drafted AI reply or quick replies | `Message.AiSuggestion` (`type` `reply` or `quick-replies`, `loading`). The person always sends. |
| Empty state visuals for users or integrations | Compose avatars, `FeaturedIcon`, and app logos in `EmptyState.Visual` (Empty State docs) |

## Charts

| Need | Use |
| --- | --- |
| A chart at a Figma height | `size` `sm` (198px), `md` (278px), `lg` (358px) on `LineChart`, `AreaChart`, `BarChart`, `ComboChart`, `PieChart`. Without `size` the plot is 320px. |
| A line compared with the previous period | A series with `dashed: true`. Every other Line Chart series is solid; never dash a series only to tell lines apart. |
| Stacked or 100% stacked areas or bars | `stacked`, `stackedPercent` |
| One highlighted bar | `BarChart highlightIndex` |
| Bars or an area with a line | `ComboChart type` `bar-line`, `stacked-bar-line` (several `barSeries`), `area-line`, `bar-area` |
| Pie, donut, or two rings | `PieChart type` `pie`, `donut` (default), `nested-donut` (`innerValue`, `ringLabels`) |
| Packed circles sized by value | `PackedBubbleChart` |
| A small trend without axes | `Sparkline` (`trend` `positive`, `negative`, `neutral`; `type` `line`, `area`, `bar`) |
| Funnel, radar, radial, heatmap, waterfall, Sankey | `FunnelChart`, `RadarChart`, `RadialChart`, `Heatmap`, `WaterfallChart`, `SankeyChart` |
| A chart inside a titled card with a legend | `ChartCard` with `ChartLegend`; a chart inside it drops its own surface. `ChartReferenceLine` draws an average or target line. |
| Widget plots | `ProportionBarChart`, `TimelineBandChart`, `RangeBarChart`, `LollipopChart`, `QuadrantChart` (`rangeSliders`) |

## Constants and helpers

To list the Figma options in your own UI (a picker, a filter, a test), use the exported lists instead of copying values: `PAYMENT_METHODS`, `INTEGRATION_ICON_NAMES`, `SOCIAL_ICON_PLATFORMS`, `SOCIAL_BUTTON_SOCIALS`, `MOBILE_APP_STORES`, `BACKGROUND_PATTERN_TYPES`, `BACKGROUND_PATTERN_DECORATIVE_TYPES`, `BACKGROUND_OVERLAY_TYPES`, `SUPER_GRAPHIC_LIFTED_LINES_VARIANTS`, `SUPER_GRAPHIC_PIXEL_INSIGHT_VARIANTS`, `SUPER_GRAPHIC_FACET_VARIANTS`, `ILLUSTRATION_SCENES`, `LYMO_MASCOT_POSES`, `LYMO_LOADING_STATES`, `FILE_TYPE_ICON_TYPES`, `CODE_SNIPPET_LANGUAGES`. `getPaymentMethodLabel`, `getIntegrationIconLabel`, and `getSocialIconLabel` return the accessible name of a mark. `closeButtonVariants` and `utilityButtonVariants` give the classes of those buttons for an element that must match them; prefer the components.

## Removed in 2.x, with the replacement

- `MessageComposer variant="medium"` → `variant="textarea"`.
- `PieChart variant` → `type="donut"` (was `outline`) or `type="pie"` (was `filled`).
- The 11 old Date picker presets → the nine Figma presets.
- `FunnelStageCard` → `FunnelChart`. `StepIndicator` → `ProgressTextWithLine`.
