# Component and token decisions

Use this reference when a requested design does not map directly to a single Nesra component or needs styling beyond its defaults. Confirm every concrete import, prop, and utility in the installed package and current docs before writing it.

## Component choice

| Situation | Next step |
| --- | --- |
| A public component covers the control and behavior | Use it with documented props and public parts. |
| Several controls form a larger section | Compose public components with ordinary React markup for the section's layout. Keep the components' own interaction behavior. |
| A page is marked upcoming | Treat it as unavailable. Check whether current public primitives can meet the need without reproducing the upcoming component's internals. |
| No public API covers a required behavior | Compose public primitives or implement the application-specific behavior accessibly. Raise a choice if the solution changes the requested design, behavior, or dependencies. |
| The user explicitly chooses another library or the host app has a binding constraint | Respect that choice or constraint while keeping Nesra usage coherent where it applies. |

Do not infer an API from a Figma name, a source file that is not exported, or a similar shadcn component. Do not import from `@nesra/ui/dist` or private source paths. For unfamiliar parts, check package exports, declarations, and component docs rather than guessing a compound API.

## Styling boundary

Use the installed package's semantic Tailwind utilities for color and other available roles. Prefer a semantic role when one exists; use raw palette values only when the design calls for a specific primitive and no suitable role applies. Do not invent generic aliases such as `primary` or `foreground`. In version 0.4.0, the published token contract supports light mode only; recheck the installed stylesheet for other versions.

Application layout classes, wrappers, spacing, and responsive composition are ordinary customization. The package exposes Unit, Gap, Padding, Margin, and semantic Spacing scales; use the appropriate scale rather than assuming all layout utilities share one naming pattern. First use documented props and public parts for component variants and states. Extensive CSS that replaces a component's visual states, overrides its internals, or depends on private selectors is a sign that the requirement may exceed the public contract. Surface that trade-off before committing to it.

The application owns accessibility that components cannot supply from context, including meaningful labels, page structure, and end-to-end keyboard flows. Check the relevant component page for what Nesra implements and what the application must provide.

## Interface composition checks

- Keep related headings and descriptions closer to each other than to adjacent controls or sections. Use the documented Nesra spacing roles. For example, a title and supporting copy can share a small gap inside a group with a larger gap to its action.
- Align an icon with the first line when its adjacent text can wrap. Check the result at a narrow viewport and with longer copy; the icon should not drift to the middle of the entire text block.
- When nesting rounded surfaces, compare the inner radius, outer radius, and padding so the corners look concentric. Choose from Nesra's documented radius and padding tokens.
- Give sticky headers or controls a visible separation from scrolling content when the design calls for one. Use a documented Nesra stroke or elevation role and inspect the scrolled state.
- For an animated controlled `Dialog`, keep `Dialog.Root` mounted while changing its `open` prop so the close transition can finish. Follow the current Dialog page for composition, focus behavior, and dismissal. Conditionally render the whole dialog only when no exit animation is needed.

For example, group a section title and description before spacing the action away:

```tsx
import { Button } from "@nesra/ui/button";
import { Text } from "@nesra/ui/text";

export function AccountSection() {
  return (
    <div className="grid gap-nesra-spacing-xl">
      <div className="grid gap-nesra-spacing-xs">
        <Text as="h2" variant="heading5">Account details</Text>
        <Text size="small">Review the information before saving.</Text>
      </div>
      <Button>Continue</Button>
    </div>
  );
}
```

Use the current Text, typography, layout, shape and elevation, and Dialog documentation for exact props and tokens. These checks guide composition; the installed package and approved design still determine the implementation.
