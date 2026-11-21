# Component and token decisions

Use this reference when a requested design does not map directly to a single Nesra component. Confirm every concrete import, prop, and utility in the installed package and current docs before writing it. The 2.x sections below apply only when the installed `@nesra/ui` is 2.x. In a 1.x project, use the installed package's exports, types, and stylesheet instead.

## Component choice

| Situation | Next step |
| --- | --- |
| A public component covers the control and behavior | Use it with documented props and public parts. |
| Several controls form a larger section | Compose public components with ordinary React markup for the section's layout. Keep the components' own interaction behavior. |
| A page is marked upcoming | Treat it as unavailable. Check whether current public primitives can meet the need without reproducing the upcoming component's internals. |
| No documented public API covers a required behavior | Describe the gap and options to the user before introducing another library or a substitute component. |
| The user explicitly chooses another library or the host app has a binding constraint | Respect that choice or constraint while keeping Nesra usage coherent where it applies. |

Do not infer an API from a Figma name, a source file that is not exported, or a similar shadcn component. Do not import from `@nesra/ui/dist` or private source paths. For unfamiliar parts, check package exports, declarations, and component docs rather than guessing a compound API.

## Component decisions in 2.x

Each row names the 2.x choice. Confirm the import and props in the installed package and the current docs before using it.

| Need | Use | Not |
| --- | --- | --- |
| A dismiss or close X anywhere (dialogs, banners, tags, panels) | `CloseButton` from `@nesra/ui/close-button`. Pass `aria-label` to name what it closes. | A ghost icon `Button` with an X |
| A segmented control that switches a value in place (Day, Week, Month) | `ButtonGroup` from `@nesra/ui/button-group` | `Tabs` |
| Sections of a page or panel | `Tabs` with the `brand`, `gray`, `minimal`, or `line` variant | `border` or `pill`, which were removed |
| A removable, selectable, or input chip | `Tag` from `@nesra/ui/tag`, or `TagsInput` from `@nesra/ui/input` | `Badge` |
| A status or label | `Badge` with one of its 11 colors: `gray`, `brand`, `error`, `warning`, `success`, `slate`, `sky`, `blue`, `purple`, `pink`, `orange`, and a `type` of `badge-color`, `badge-modern`, or `pill-color` | `neutral`, `info`, `indigo` (use `brand`), or the removed `variant` (indicator, status, course, xp, level) |
| A metric with a trend | The KPI family from `@nesra/ui/metric`: `Kpi`, `KpiGroup`, `KpiDetailRow`, `KpiComposition`, `TrendChip` | `MetricTile`, `MetricChartCard`, `FunnelStageCard`, `TrendIndicator`, and the other removed Metric parts |
| A short hint on hover or focus | `Tooltip`, which is light. Pass `arrow` for the arrow. | Forcing a dark tooltip or inverse text |
| Rich or interactive floating content | `Popover` | `Tooltip` with buttons or long text |
| Transient success, error, info, or warning | `Toast` (keep the feedback rule below) | A banner or dialog |
| A persistent page or section message | `InformationBanner` `variant="floating"` (default) or `"full-width"`, `color` `default`, `gray`, `brand`, `error`, `warning`, or `success` | A Toast, or the removed `basic`, `gradient`, `soft` variants and categorical colors |
| A decision the user must make before continuing, such as deleting | `Dialog` | A Toast or banner |
| A page title area with description and actions | `PageHeader` from `@nesra/ui/page-header` | Hand-built heading markup |
| Field height | `size` `"sm"`, `"md"` (default, 38px), or `"lg"` on `Input`, `InputGroup`, `TagsInput`, `Select`, `Combobox`, `MultiSelect`. Pass `label`, `description`, and `error` to the control itself: at `lg` they render in Text md (16/24), so a `lg` field with label and hint is 106px. A hand-composed `Field` always uses Text sm. | The native numeric `size` attribute, custom heights, or a text class on the label |
| A multiline field | `Textarea` `size` `"sm"` (110px minimum) or `"md"` (default, 128px minimum). Use `rows` for more lines. | A `min-h-*` class to recreate the 1.x 80px height |
| Avatar size | The Figma names `xs` 24px, `sm` 32px, `md` 40px, `lg` 48px, `xl` 56px, `2xl` 64px | The 1.x names, which shifted one step |
| Button size | `xs` 26px, `sm` 34px, `md` 38px (default), `lg` 42px, `xl` 50px. Button sizes child icons itself. | A `size` prop on the icon |
| Loading placeholder for content that AI is generating | `Skeleton tone="ai"` (the spectrum fill) | The removed `tone="teal"` or a custom gradient |
| An avatar without an image | `Avatar` with `initials` or the default icon. Its fallback is one neutral gray in 2.x. The `border-secondary` line is drawn inside the circle (0.5px `xs`, 0.75px `sm`, 1px from `md`); a photo fills the circle with a faint 0.5px edge. | Picking a color per person, or adding a `border` or `ring` class to `Avatar` |
| Table row height | The default, 32px (Figma Compact). For 40px rows wrap the table in `data-nesra-table-density="standard"`; `density="comfortable"` is 48px. | Custom row heights |
| A dropdown menu width | The `DropdownMenu.Content` default, 240px, for Figma Button advanced, Icon advanced, Search, Account button, avatar, and card sm/md menus and the Table menus. Otherwise pass the Figma width in `className`: `w-54` (216px) for Button simple, Icon simple, and Integrations; `w-50` (200px) for Account card xs and Submenu; `w-40` (160px) for Button link; `w-62` (248px) for Account breadcrumb. `Sidebar.PlatformMenu` already sets 224px. | Another width class, or leaving 240px on a Figma 216px menu |
| A dialog width | `Dialog.Content` `size` `"400"` (default), `"480"`, `"544"`, `"560"`, `"640"`, `"688"`, `"720"`, or `"800"`, the Figma Modal widths | `sm` or `md`, which were removed, or a custom width class when a size fits |
| A filter button for a toolbar | `Filters.Trigger` with `count` | A `Button` restyled to 36px |
| A table whose filter row opens and closes | `Table.FilterRow open` toggled by a `Table.ToolbarButton` with `aria-expanded` and `aria-controls`, and `layout="fixed"` with a `<colgroup>` so columns do not move | `hidden` toggling in `layout="auto"`, which shifts column widths |
| A spreadsheet-style editable grid | Copy the Data grid example ([Data grid in 2.x](data-grid.md)) | Importing a `DataGrid` from `@nesra/ui`, which does not exist, or adding another grid library |
| A search field with a shortcut hint | `InputGroup.Shortcut` inside a search `InputGroup` | A hand-built `kbd` |
| The search in a Filters bar or Page header (Figma Select Type=Search) | `InputGroup size="sm"` with a `Search` icon in `InputGroup.Addon`, 280px wide; next to `size="sm"` Selects the Filters bar is 36px | A `md` `Input`, which makes the bar 38px |
| Clearing a search field (Figma Select Type=Search) | `InputGroup.Clear` after `InputGroup.Input` (and `InputGroup.Shortcut`), with an `aria-label` such as "Clear search" (the default name is the localized `Clear` message key). It is a 16px `x-close` in `icon-tertiary` that renders only while the input has a value, never in a disabled group or on a disabled or read-only input, and `InputGroup.Shortcut` hides meanwhile. It empties controlled and uncontrolled inputs through a native input event, so `onChange` and `onValueChange` receive `""`, then focuses the input; `onClick` with `preventDefault()` keeps the value. No state is needed for an uncontrolled search. `Input`, `InputGroup.Input`, `CommandMenu.Input`, and `DropdownMenu.Search` with `type="search"` hide the browser's own clear button. A search with a trailing submit button (Has Button) has no clear button. | A hand-built `CloseButton` with its own state, the browser's native × (it is not a Nesra icon), a text `×` or `x` character, or a clear button next to the shortcut |
| A phone number after a country-code addon such as 🇮🇩 `+62` | `InputGroup.Input` with `type="tel"`, `inputMode="numeric"`, `autoComplete="tel-national"`, and placeholder `812-0000-0000`, plus `onChange`, `onCompositionEnd`, and `onKeyDown` handlers copied from the InputGroup Variants docs example. They keep digits only, drop a pasted leading `+62` and any leading `0`, group the digits 3-4-rest as `812-3456-7890` (Figma), keep the caret after the same digit, and make Backspace or Delete next to a hyphen remove the digit beyond it. There is no maximum length. The submitted value keeps the hyphens; send `"+62" + value.replace(/\D/g, "")`. `InputGroup.Input` itself does not filter or format. | Letters, spaces, `+`, or a leading `0` in the value, a `maxLength`, or only `type="tel"`, which accepts any character |
| Progress through a few labelled steps | `ProgressTextWithLine` from `@nesra/ui/indicator`, or `Stepper` for numbered steps | The removed `StepIndicator` |
| A term with a definition on hover | `RichTooltip`, or `AiInlineInsight.Why` for an AI insight | The removed `TooltipTrigger` |
| Calendar event colors | `gray`, `brand`, `green`, `blue`, `purple`, `pink`, `orange`, `yellow` | `indigo`, which was removed |
| Chart colors | `series-1` to `series-8`, `heat-1` to `heat-4`, `positive`, `negative`, `neutral`, and the `series` and `heat` palettes. `ChartBand` and `ChartMarker` take `neutral` or `ai`. | The removed categorical names (`blue`, `teal`, `violet`, …), `mono-*`, `blue-*` |
| A line chart with several series | Solid lines for every series, told apart by `series-*` color and the legend. Dash only a comparison series (`dashed: true`, Figma Dashed comparison). | Dashes or markers to separate ordinary series |
| A Figma Lymo component not listed here | The matching row in [Lymo components in 2.x](lymo-components.md) | Building it by hand from a Figma screenshot |
| AI insights that are still generating (Figma AI insight banner Loading) | `AiInsights state="loading"`. It renders the 24px `LymoLoader` (`processing`) and "Generating insights" in Text xs Medium `text-tertiary` in the header, with no button, and with `open` three `Skeleton tone="ai"` lines. Lymo is `aria-hidden`, the label sits in an `aria-busy` wrapper, and the polite status region announces the change. To translate the label and the announcement, override the `Generating insights` message key; `Generating…` is no longer rendered. Other AI loading states (`AiInlineInsight.Loading`, `AiExplanationPopover`, `AiDriverBreakdown`, `AiRecommendationCard` `loading`) stay skeleton-only. | A disabled "Generating…" `Button` with a spinner, a `LoadingIndicator`, a second `LymoLoader` beside it, or a hand-placed Lymo in another AI component |
| An AI result that failed to load | `AiInlineInsight.Error`, or `state="error"` with `onRetry` on `AiInsights`, `AiExplanationPopover`, and `AiDriverBreakdown` | A custom error line |

## Figma parity: values you should not compensate for

`@nesra/ui` 2.x matches these Figma Lymo values (Irfan's QA, November 2026). If a screen looks 1 or 2px off next to an older build, update the package instead of adding classes:

- Figma strokes sit inside the frame, so components subtract the 1px border from their padding: `Badge` color and modern 20/22/24px, Pill color 18/20/22px, `BadgeGroup` 26/28px (modern 28/30px), `TrendChip` `variant="badge"` 20px (QA-011), `MobileTabBar` 72px, `Table.Toolbar` 58px (QA-027), and the `FileInput` `lg` drop zone 126px (QA-025).
- `Tag` uses a 6px radius at every size (QA-012). `MultiSelect` pads `md` and `lg` 16px, while `Select` keeps 12px (QA-016).
- `Calendar` at 640px and wider uses a 20px radius and `bg-primary` for the panel, header, column headers, and cells (#0A0A0A in Dark). The compact layout below 640px keeps 12px and `bg-primary_alt` (QA-023).
- `Toast.Close` is the Figma Notification `Buttons/Button close X` sm: a bare 24px button (4px padding, 8px radius) with a 16px `icon-tertiary` X and no fill or border at rest, 8px from the top and 12px from the right of the card on Desktop and Mobile (Type=Progress indicator: 4px and 20px). The title column keeps 32px right padding, so the title stays 12px clear of the X. On Mobile, a toast with a tile or avatar runs the title to the 16px card padding. Hover fills `bg-primary_hover` and darkens the X to `icon-tertiary_hover`. Keyboard focus shows one `shadow-nesra-focus-ring`. Showing a toast never moves focus; F6 reaches the Notifications region and Tab reaches the X. Use the default `Toast.List` composition, or place `<Toast.Close className="absolute top-nesra-spacing-md right-nesra-spacing-lg" />` in a custom one. Do not restyle it.
- `InformationBanner` `default` and `gray` use `border-primary` (QA-022). `AiRecommendationCard` `size="full"` pads 16px on `bg-primary` (QA-024). `FileInput` `lg` uses `border-primary` (QA-025). `Message.Author` uses `text-secondary` (QA-026).

## Feedback patterns

Prefer a toast over a block-style feedback message for transient success, error, info, and warning states. Keep actionable validation near the affected field or control, and use a blocking dialog only when the user must make a decision before continuing. Confirm the toast API in the installed package and current docs before using it.

## Styling boundary

Use documented Nesra semantic tokens for application surfaces and layout where needed. Do not substitute another palette, typography scale, or visual system, and do not invent generic aliases such as `primary` or `foreground`.

Nesra tokens carry Light and Dark values. Light is the default. A `dark` class or `data-theme="dark"` on `<html>` or a container switches that subtree to Dark, and every semantic role and component follows it. Do not write `dark:` variants in application code. The application owns the theme switch and any saved preference.

In 2.x, every token is a `--nesra-*` CSS variable named after its Figma 2.0 variable, with a Tailwind utility of the same name, for example `bg-nesra-bg-primary`, `text-nesra-text-secondary`, `border-nesra-border-secondary`, `p-nesra-spacing-xl`, `rounded-nesra-xl`, and `shadow-nesra-xs`. Follow these rules:

- Text style classes (`type-nesra-*`) set no color. Add a text role next to them, usually `text-nesra-text-primary`. `Text` adds the color for you.
- `type-nesra-*` classes are plain CSS classes in the package stylesheet, not Tailwind utilities. A Tailwind variant on one (`md:type-nesra-text-lg-medium`, `group-data-[size=sm]/x:type-nesra-text-xs-medium`, `[&_h1]:type-nesra-text-lg-semibold`) or the important modifier (`type-nesra-text-sm-medium!`, `!type-nesra-text-sm-medium`) compiles to no CSS, so the text silently keeps its inherited style. Pick one complete class per state in TypeScript, or use the Tailwind font utilities (`text-nesra-text-sm`, `font-nesra-medium`) when a variant is needed. `cn` already resolves a `type-nesra-*` class against `text-*`, `font-*`, `leading-*`, and `tracking-*` overrides, so `!` is never needed.
- Nesra UI does not fetch or bundle fonts. Load Manrope (400, 500, 600, 700) and Source Code Pro (400, 500) once in the application entry point, for example with `@fontsource/manrope` and `@fontsource/source-code-pro` (`import "@fontsource/manrope/500.css"` per weight) or a Google Fonts `<link>`. Without them, text falls back to the system font stack. The Installation page has both setups.
- Put global focus styles in the app stylesheet, such as `:focus-visible { outline: … }` or `input:focus-visible`, inside `@layer base`. Nesra fields draw one focus ring on the field wrapper and set `outline-none` on the text field inside it, for example in `TagsInput`, `Combobox`, `MultiSelect`, and `DateTimeInput`. Tailwind puts `outline-none` in `@layer utilities`, and an unlayered rule beats every layered rule whatever its specificity, so an unlayered global focus rule draws a second outline inside the field's ring. Do not override the outline of the text field inside a Nesra field. The Installation page section "Keep global focus styles in a layer" has the setup and a check.
- A gray, bordered box around a Nesra close X (about 28px instead of 24px) is the browser's default `<button>` style, not a Nesra state: the application loads no Tailwind Preflight or base button reset where that button renders. Portaled parts (Toast, Dialog, Sheet) render under `<body>`, outside any reset scoped to a page section. `Toast.Close` sets `border-0 bg-transparent` itself (design-system-nesra #279). The `CloseButton` inside `Dialog` and `Sheet` does not yet, so keep Preflight (`@import "tailwindcss"`) or a `button { background: transparent; border: 0 solid; }` reset in `@layer base` for the whole document. Do not add classes to the close button to hide the box.
- The radius scale moved one step from 1.x: `rounded-nesra-md` is 8px and `rounded-nesra-lg` is 10px.
- Do not use raw palette values (`bg-blue-500`, hex colors) or arbitrary values (`p-[13px]`) when a token or the Tailwind scale expresses the intent. Use the `utility-*` ramps for categorical color.
- Do not use the `--nesra-v2-*` names from the 2.0 pre-release or the 1.x legacy tokens (`categorical-violet-*`, `categorical-teal-*`, `chart-mono-*`, `chart-blue-*`, `stroke-width-1-5`). They were removed from 2.x.

### Before you pick a color

Run this check against the Color language section of the docs Colors page (https://ui.nesra.ai/colors/#color-language). It is a checklist, not a copy of that page.

- Give each color one meaning: neutral for content and structure, indigo for active, selected, in progress, or clickable, the spectrum for an AI result or a special moment.
- Never use the spectrum for selection or focus, status, data categories, long text, form controls, or navigation icons. Keep at most one macro spectrum moment per screen.
- Never use Rose or Orange alone as an accent.
- Never replace a status color (error, warning, success) or a data color (`chart-series-*`) with a brand color.
- Prefer a component that already applies the recipe (Checkbox, Tabs, Filters, AI components) over coloring a custom element.

Find token names on the Colors, Typography, Layout, and Shape and elevation pages rather than guessing them. This skill does not copy the token catalog.

Application layout, wrappers, spacing, and responsive composition are the intended ways to assemble an interface. Use documented props and public parts for component variants and states. Do not extend Nesra components or override their internal styles, colors, typography, or visual states with custom CSS or private selectors. If composition and the public API cannot meet a requirement, explain the gap before implementing that part.

The application owns accessibility that components cannot supply from context, including meaningful labels, page structure, and end-to-end keyboard flows. Check the relevant component page for what Nesra implements and what the application must provide.

## Interface composition checks

- Keep related headings and descriptions closer to each other than to adjacent controls or sections. Use the documented Nesra spacing roles. For example, a title and supporting copy can share a small gap inside a group with a larger gap to its action.
- Align an icon with the first line when its adjacent text can wrap. Check the result at a narrow viewport and with longer copy; the icon should not drift to the middle of the entire text block.
- When nesting rounded surfaces, compare the inner radius, outer radius, and padding so the corners look concentric. Choose from Nesra's documented radius and padding tokens.
- Give sticky headers or controls a visible separation from scrolling content when the design calls for one. Use a documented Nesra border or elevation role and inspect the scrolled state in Light and Dark.
- For an animated controlled `Dialog`, keep `Dialog.Root` mounted while changing its `open` prop so the close transition can finish. Follow the current Dialog page for composition, focus behavior, and dismissal. Conditionally render the whole dialog only when no exit animation is needed.

For example, group a section title and description before spacing the action away (2.x names):

```tsx
import { Button } from "@nesra/ui/button";
import { Text } from "@nesra/ui/text";

export function AccountSection() {
  return (
    <div className="grid gap-nesra-spacing-xl">
      <div className="grid gap-nesra-spacing-xs">
        <Text as="h2" variant="heading5">
          Account details
        </Text>
        <Text size="small" className="text-nesra-text-tertiary">
          Review the information before saving.
        </Text>
      </div>
      <Button>Continue</Button>
    </div>
  );
}
```

Use the current Text, typography, layout, shape and elevation, and Dialog documentation for exact props and tokens. These checks guide composition; the installed package and Nesra design system determine the implementation.
