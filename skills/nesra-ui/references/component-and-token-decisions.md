# Component and token decisions

Use this reference when a requested design does not map directly to a single Nesra component or needs styling beyond its defaults. Confirm every concrete import, prop, and utility in the installed package and current docs before writing it.

## Component choice

| Situation | Next step |
| --- | --- |
| A public component covers the control and behavior | Use it with documented props and public parts. |
| Several controls form a larger section | Compose public components with ordinary React markup for the section's layout. Keep the components' own interaction behavior. |
| A page is marked upcoming | Treat it as unavailable. Check whether current public primitives can meet the need without reproducing the upcoming component's internals. |
| No documented public API covers a required behavior | Describe the gap and options to the user before introducing another library or a substitute component. |
| The user explicitly chooses another library or the host app has a binding constraint | Respect that choice or constraint while keeping Nesra usage coherent where it applies. |

Do not infer an API from a Figma name, a source file that is not exported, or a similar shadcn component. Do not import from `@nesra/ui/dist` or private source paths. For unfamiliar parts, check package exports, declarations, and component docs rather than guessing a compound API.

## Styling boundary

Use the documented Nesra semantic Tailwind utilities for color and other available roles. Prefer a semantic role when one exists; use raw palette values only when the design calls for a specific primitive and no suitable role applies. Do not invent generic aliases such as `primary` or `foreground`. The current public color contract is Light Mode only.

Application layout classes, wrappers, spacing, and responsive composition are ordinary customization. First use documented props and public parts for component variants and states. Extensive CSS that replaces a component's visual states, overrides its internals, or depends on private selectors is a sign that the requirement may exceed the public contract. Surface that trade-off before committing to it.

The application owns accessibility that components cannot supply from context, including meaningful labels, page structure, and end-to-end keyboard flows. Check the relevant component page for what Nesra implements and what the application must provide.
