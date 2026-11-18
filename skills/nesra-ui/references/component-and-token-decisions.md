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
| Sections of a page or panel | `Tabs` with the `brand`, `gray`, `minimal`, or `line` variant | The deprecated `border` or `pill` variants |
| A removable, selectable, or input chip | `Tag` from `@nesra/ui/tag`, or `TagsInput` from `@nesra/ui/input` | `Badge` |
| A status or label | `Badge` with one of its 12 colors: `gray`, `brand`, `error`, `warning`, `success`, `slate`, `sky`, `blue`, `indigo`, `purple`, `pink`, `orange` | `neutral` or `info`, which no longer exist |
| A metric with a trend | The KPI family from `@nesra/ui/metric`: `Kpi`, `KpiGroup`, `TrendChip` | The older Metric cards for new layouts |
| A short hint on hover or focus | `Tooltip`, which is light. Pass `arrow` for the arrow. | Forcing a dark tooltip or inverse text |
| Rich or interactive floating content | `Popover` | `Tooltip` with buttons or long text |
| Transient success, error, info, or warning | `Toast` (keep the feedback rule below) | A banner or dialog |
| A persistent page or section message | `InformationBanner` `variant="floating"` or `"full-width"` | A Toast |
| A decision the user must make before continuing, such as deleting | `Dialog` | A Toast or banner |
| A page title area with description and actions | `PageHeader` from `@nesra/ui/page-header` | Hand-built heading markup |
| Field height | `size` `"sm"`, `"md"` (default, 38px), or `"lg"` on `Input`, `InputGroup`, `Select`, `Combobox` | The native numeric `size` attribute or custom heights |
| Avatar size | The Figma names `xs` 24px, `sm` 32px, `md` 40px, `lg` 48px, `xl` 56px, `2xl` 64px | The 1.x names, which shifted one step |
| Button size | `xs` 26px, `sm` 34px, `md` 38px (default), `lg` 42px, `xl` 50px. Button sizes child icons itself. | A `size` prop on the icon |

## Feedback patterns

Prefer a toast over a block-style feedback message for transient success, error, info, and warning states. Keep actionable validation near the affected field or control, and use a blocking dialog only when the user must make a decision before continuing. Confirm the toast API in the installed package and current docs before using it.

## Styling boundary

Use documented Nesra semantic tokens for application surfaces and layout where needed. Do not substitute another palette, typography scale, or visual system, and do not invent generic aliases such as `primary` or `foreground`.

Nesra tokens carry Light and Dark values. Light is the default. A `dark` class or `data-theme="dark"` on `<html>` or a container switches that subtree to Dark, and every semantic role and component follows it. Do not write `dark:` variants in application code. The application owns the theme switch and any saved preference.

In 2.x, every token is a `--nesra-*` CSS variable named after its Figma 2.0 variable, with a Tailwind utility of the same name, for example `bg-nesra-bg-primary`, `text-nesra-text-secondary`, `border-nesra-border-secondary`, `p-nesra-spacing-xl`, `rounded-nesra-xl`, and `shadow-nesra-xs`. Follow these rules:

- Text style classes (`type-nesra-*`) set no color. Add a text role next to them, usually `text-nesra-text-primary`. `Text` adds the color for you.
- The radius scale moved one step from 1.x: `rounded-nesra-md` is 8px and `rounded-nesra-lg` is 10px.
- Do not use raw palette values (`bg-blue-500`, hex colors) or arbitrary values (`p-[13px]`) when a token or the Tailwind scale expresses the intent. Use the `utility-*` ramps for categorical color.
- Do not use the `--nesra-v2-*` names from the 2.0 pre-release or the deprecated legacy tokens (`categorical-violet-*`, `categorical-teal-*`, `chart-mono-*`, `chart-blue-*`) in new code.

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
