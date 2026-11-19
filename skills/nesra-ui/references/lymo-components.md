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
| Avatar with a count or avatar badge, a name and email, or a large profile photo | `Avatar` `status="count"` or `"avatar"`, `Avatar.LabelGroup`, `Avatar.ProfilePhoto` (`verified`). `initials` shows at most two characters and is never truncated, so pass two letters. |
| A range or single value slider | `Slider` (`orientation="vertical"` for a Y range) |
| A selectable card list | `RadioGroup` `variant` (`radio-button`, `checkbox`, `icon-simple`, `avatar`, `payment-icon`, `icon-card`) |
| A loading spinner or dots | `LoadingIndicator` (`line-simple`, `line-spinner`, `dot-circle`) |
| A section or content divider | `Divider` (`container` for the Figma Section divider) |
| Code with copy and syntax colors | `CodeSnippet` (`language`, `CODE_SNIPPET_LANGUAGES`) |
| A video | `VideoPlayer` |
| A rich text field | `TextEditorEditor` from `@nesra/ui/text-editor-tiptap`; `TextEditor` is the frame and toolbar without an engine |
| Brand, payment, integration, and store marks | `SocialIcon`, `PaymentMethodIcon`, `IntegrationIcon`, `SocialButton`, `SocialButtonGroup`, `MobileAppStoreBadge`. `SocialButton social` is `google`, `facebook`, or `apple` (the Figma Social button group); Apple `brand` is a black button. |
| Decorative backgrounds and illustrations | `BackgroundPattern`, `BackgroundPatternDecorative`, `BackgroundOverlay`, `SuperGraphicLiftedLines`, `SuperGraphicPixelInsight`, `SuperGraphicFacet`, `Illustration`, `LymoMascot`, `LymoLoader`, `LymoProgressBar`, `LymoLoadingStatus`, `EmptyState.Illustration` |

## Application components

| Need | Use |
| --- | --- |
| Switch between products in the sidebar | `Sidebar.PlatformSwitcher` with `Sidebar.PlatformMenu` and `Sidebar.PlatformLogo` |
| A product rail or a mobile bottom bar | `AppRail`, `MobileTabBar` |
| A card at the bottom of the sidebar (usage, upgrade, onboarding, support) | `Sidebar.FeaturedCard` with `Sidebar.FeaturedCardTitle`, `Sidebar.FeaturedCardDescription`, `Sidebar.FeaturedCardActions`. Compose the Figma types from other components. |
| The application top bar (Figma Header navigation) | `HeaderNavigation` from `@nesra/ui/header-navigation` with `type` `simple`, `dual-tier`, `tabs`, `centered`, or `plane` (`HEADER_NAVIGATION_TYPES`). Compose `HeaderNavigation.Bar` with `.Logo`, `.Nav` (requires `aria-label`) and `.NavItem` (`current`), `.Actions`, `.NavButton` (`label`, `icon`, `count` for the unread counter), and `.MenuButton`. Add `.SecondaryBar` for the second row (Figma Desktop open); omit it to hide that row. Plane uses `.Brand` (`mark`), `.Workspace`, `.Tagline`, `.Title` (`parent`), and `.MobileActions`. The header switches to Mobile below 896px of its own width; do not add a breakpoint prop or duplicate the bar. |
| The mobile menu of the header | `HeaderNavigation.Drawer` (`label`), opened by `.MenuButton`; control it with `open`, `defaultOpen`, `onOpenChange` on the root. Fill it with Sidebar parts (`Sidebar.Content`, `Sidebar.Menu`, `Sidebar.MenuButton`, `Sidebar.Collapsible` with `Sidebar.MenuSub`, `Sidebar.Footer`, `Sidebar.AccountCard`); the drawer provides their context. |
| A KPI group with a title, an AI insight, and a source | `KpiGroup.Header`, `KpiGroup.Insight`, `KpiGroup.Source` |
| Rich table cells, toolbar, filter row, summary, footer | `Table.*Cell` parts with `Table.LeadAction`, `Table.StateIcon`, `Table.PriorityIcon`, `Table.Toolbar`, `Table.ToolbarButton`, `Table.FilterRow`, `Table.FilterCell`, `Table.SummaryCell`, `Table.FooterBar`, `Table.StatusBar` |
| A Filter button that opens filters | `Table.ToolbarButton` has no behavior of its own. Either toggle `Table.FilterRow` with `onClick` and set `aria-expanded` (not only `active`), or make it the trigger of a `Popover` or `DropdownMenu` with `render={<Table.ToolbarButton icon={…}>Filter</Table.ToolbarButton>}`. Show the number of active filters with `count`. Filtering and data loading stay in the app. |
| Work items as a grouped list or a Kanban board | `Table.List` with `Table.ListGroupHeader` (`name`, `count`, `icon`, `as`, `onAdd` with `addLabel`) and `Table.ListRow` (`code`, `title`, `properties`, `assignee`, `actions`); `Table.BoardCard` (`code`, `title`, `platform`, `properties`, `assignee`; with `onClick` it is one button) in your own board columns. Use the list or board for items people scan or move by status; use `Table` when they compare values across columns, sort, or select rows. |
| Pages, dots, or a carousel | `Pagination` (`type`, `breakpoint`), `Pagination.DotGroup`, `Carousel` |
| A tree list | `TreeView` (not `TableTree`, which is a table) |
| Account, search, or integrations in a dropdown | `DropdownMenu.AccountTrigger`, `DropdownMenu.AccountItem`, `DropdownMenu.Search`, `DropdownMenu.Footer` |
| Notification types | `Toast` `type` `avatar`, `image`, `progress`, `ai` |
| Page header styles | `PageHeader variant` (9 Figma styles) with `PageHeader.Banner`, `.Avatar`, `.Search`, `.Count` |
| A date and time field | `DateTimeInput` |
| A date with a Today button, or a date with time slots | `DatePicker` (single) and `availableTimes` with `onTimeChange` |
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

To list the Figma options in your own UI (a picker, a filter, a test), use the exported lists instead of copying values: `PAYMENT_METHODS`, `INTEGRATION_ICON_NAMES`, `SOCIAL_ICON_PLATFORMS`, `SOCIAL_BUTTON_SOCIALS`, `HEADER_NAVIGATION_TYPES`, `MOBILE_APP_STORES`, `BACKGROUND_PATTERN_TYPES`, `BACKGROUND_PATTERN_DECORATIVE_TYPES`, `BACKGROUND_OVERLAY_TYPES`, `SUPER_GRAPHIC_LIFTED_LINES_VARIANTS`, `SUPER_GRAPHIC_PIXEL_INSIGHT_VARIANTS`, `SUPER_GRAPHIC_FACET_VARIANTS`, `ILLUSTRATION_SCENES`, `LYMO_MASCOT_POSES`, `LYMO_LOADING_STATES`, `FILE_TYPE_ICON_TYPES`, `CODE_SNIPPET_LANGUAGES`. `getPaymentMethodLabel`, `getIntegrationIconLabel`, and `getSocialIconLabel` return the accessible name of a mark. `closeButtonVariants` and `utilityButtonVariants` give the classes of those buttons for an element that must match them; prefer the components.

## Light and Dark

Every component follows the page theme through its tokens. Do not add `dark:` classes or swap colors yourself.

- `data-theme` is reserved for the Light and Dark token scope. `BadgeGroup` exposes its Figma Theme as `data-badge-theme`; select on that, never on `data-theme`, for a Badge group style.
- The Avatar fallback and `Avatar.Group` count use `bg-tertiary`, which turns dark gray in Dark.
- Received `Message.Bubble`s are `bg-secondary`, sent ones `utility-brand-50`; both turn dark in Dark.
- In Storybook, the Docs tab scopes each story's theme to the story (`data-slot="sb-theme-scope"`), so a Dark story no longer darkens the page. A Dark story whose content renders in a portal (popover, dialog, sheet, drawer, toast) sets `parameters.docs.story = { inline: false, height }`, so the portal stays dark.

## Removed in 2.x, with the replacement

- `MessageComposer variant="medium"` → `variant="textarea"`.
- `PieChart variant` → `type="donut"` (was `outline`) or `type="pie"` (was `filled`).
- The 11 old Date picker presets → the nine Figma presets.
- `FunnelStageCard` → `FunnelChart`. `StepIndicator` → `ProgressTextWithLine`.
