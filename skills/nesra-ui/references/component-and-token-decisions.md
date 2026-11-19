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
| Field height | `size` `"sm"`, `"md"` (default, 38px), or `"lg"` on `Input`, `InputGroup`, `Select`, `Combobox` | The native numeric `size` attribute or custom heights |
| Avatar size | The Figma names `xs` 24px, `sm` 32px, `md` 40px, `lg` 48px, `xl` 56px, `2xl` 64px | The 1.x names, which shifted one step |
| Button size | `xs` 26px, `sm` 34px, `md` 38px (default), `lg` 42px, `xl` 50px. Button sizes child icons itself. | A `size` prop on the icon |
| Loading placeholder for content that AI is generating | `Skeleton tone="ai"` (the spectrum fill) | The removed `tone="teal"` or a custom gradient |
| An avatar without an image | `Avatar` with `initials` or the default icon. Its fallback is one neutral gray in 2.x. | Picking a color per person |
| Table row height | The default, 32px (Figma Compact). For 40px rows wrap the table in `data-nesra-table-density="standard"`; `density="comfortable"` is 48px. | Custom row heights |
| A dialog width | `Dialog.Content` `size` `"400"` (default), `"480"`, `"544"`, `"560"`, `"640"`, or `"688"`, the Figma Modal widths | `sm` or `md`, which were removed, or a custom width class when a size fits |
| A filter button for a toolbar | `Filters.Trigger` with `count` | A `Button` restyled to 36px |
| A search field with a shortcut hint | `InputGroup.Shortcut` inside a search `InputGroup` | A hand-built `kbd` |
| Progress through a few labelled steps | `ProgressTextWithLine` from `@nesra/ui/indicator`, or `Stepper` for numbered steps | The removed `StepIndicator` |
| A term with a definition on hover | `RichTooltip`, or `AiInlineInsight.Why` for an AI insight | The removed `TooltipTrigger` |
| Calendar event colors | `gray`, `brand`, `green`, `blue`, `purple`, `pink`, `orange`, `yellow` | `indigo`, which was removed |
| Chart colors | `series-1` to `series-8`, `heat-1` to `heat-4`, `positive`, `negative`, `neutral`, and the `series` and `heat` palettes. `ChartBand` and `ChartMarker` take `neutral` or `ai`. | The removed categorical names (`blue`, `teal`, `violet`, …), `mono-*`, `blue-*` |
| An AI result that failed to load | `AiInlineInsight.Error`, or `state="error"` with `onRetry` on `AiInsights`, `AiExplanationPopover`, and `AiDriverBreakdown` | A custom error line |

## Feedback patterns

Prefer a toast over a block-style feedback message for transient success, error, info, and warning states. Keep actionable validation near the affected field or control, and use a blocking dialog only when the user must make a decision before continuing. Confirm the toast API in the installed package and current docs before using it.

## Styling boundary

Use documented Nesra semantic tokens for application surfaces and layout where needed. Do not substitute another palette, typography scale, or visual system, and do not invent generic aliases such as `primary` or `foreground`.

Nesra tokens carry Light and Dark values. Light is the default. A `dark` class or `data-theme="dark"` on `<html>` or a container switches that subtree to Dark, and every semantic role and component follows it. Do not write `dark:` variants in application code. The application owns the theme switch and any saved preference.

In 2.x, every token is a `--nesra-*` CSS variable named after its Figma 2.0 variable, with a Tailwind utility of the same name, for example `bg-nesra-bg-primary`, `text-nesra-text-secondary`, `border-nesra-border-secondary`, `p-nesra-spacing-xl`, `rounded-nesra-xl`, and `shadow-nesra-xs`. Follow these rules:

- Text style classes (`type-nesra-*`) set no color. Add a text role next to them, usually `text-nesra-text-primary`. `Text` adds the color for you.
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
