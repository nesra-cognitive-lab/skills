# Incremental adoption: 2.x next to an older release

Use this when an app still renders an older `@nesra/ui` release (for example 0.9 in the marketing app) and moves to 2.x one area at a time. Source: design-system-nesra #302 (ENG-1242), docs page "Incremental adoption" (`/incremental-adoption/`). It needs a 2.x package that contains `@nesra/ui/styles/scoped` and `NesraScope`. Check the installed package's `package.json` exports first.

## When to use `styles/scoped`

| The app renders | Import |
| --- | --- |
| Only 2.x | `@nesra/ui/styles` (the default). Never `styles/scoped`. |
| An older release outside the migrated areas | `@nesra/ui-v2/styles/scoped` (through the 2.x alias), plus the older release's own stylesheet, unchanged. |

Import exactly one 2.x stylesheet. `@nesra/ui/styles/tailwind` and `@nesra/ui/styles.css` are aliases of `@nesra/ui/styles`, not of the scoped file.

What the scoped stylesheet does:

- Every 2.x `--nesra-*` token is declared on `[data-nesra-ui="2"]`, not on `:root`. Outside a `NesraScope` the 2.x tokens do not exist.
- 2.x global rules (pinned table edges, input autofill, chart, Lymo and file upload motion), the data-attribute blocks (`data-nesra-table-density`, `data-nesra-kpi-*`, `data-nesra-product`), `type-nesra-*`, and `scrollbar-nesra*` work only inside the scope.
- `@theme inline` and `@keyframes` are identical to `@nesra/ui/styles`, so `bg-nesra-bg-primary` and every other utility compile the same.

Global stylesheet of the app, scoped import before Tailwind, one `@source` per release:

```css
@import "@nesra/ui-v2/styles/scoped";
@import "@nesra/ui/styles"; /* the older release, unchanged */
@import "tailwindcss";

@source "../node_modules/@nesra/ui/dist/**/*.js";
@source "../node_modules/@nesra/ui-v2/dist/**/*.js";
```

Without the second `@source`, 2.x components render unstyled. If Tailwind is imported in a shared package stylesheet, put the scoped import first in the app's entry stylesheet.

## Imports while both releases are installed

`package.json`: keep `"@nesra/ui"` on the older release and add `"@nesra/ui-v2": "npm:@nesra/ui@^2.0.0"` (or a `file:` tarball made with `pnpm --filter @nesra/ui pack`).

- Code in a migrated area imports only `@nesra/ui-v2/<component>`, including `@nesra/ui-v2/nesra-scope`.
- Code outside the migrated areas keeps `@nesra/ui/<component>`. Do not change those imports while migrating another area.
- Never import both releases in one file, and never render an older component inside a `NesraScope` (it would read 2.x token values) or a 2.x component outside one (it finds no tokens).
- The app enforces this per folder with ESLint `no-restricted-imports`. Follow the rule the folder has. Do not disable it.

## Every 2.x component of a migrated area is inside `NesraScope`

```tsx
import { Button } from "@nesra/ui-v2/button";
import { NesraScope } from "@nesra/ui-v2/nesra-scope";

export function AdsPage() {
  return (
    <NesraScope className="flex min-h-full flex-col">
      <Button>Create campaign</Button>
    </NesraScope>
  );
}
```

- `NesraScope` renders `<div data-nesra-ui="2">`, children first and a portal root (`[data-slot="nesra-scope-portal"]`) last. It forwards `className`, `ref`, and other `div` props. Use `render={<main aria-label="Ads" />}` to keep a landmark element.
- Put it at the top of the migrated area: the route component, or the app layout once the layout is migrated. One scope per area is enough. Nested scopes are allowed and change nothing.
- Do not hand-write `data-nesra-ui="2"` on a div instead of using `NesraScope`: popups would then still portal to `<body>` and lose the 2.x tokens.

### Popups

Inside a `NesraScope`, Dialog, Sheet, Popover, Select, Combobox, MultiSelect, DropdownMenu, Tooltip, RichTooltip, Toast, CommandMenu, ColorPicker, TextEditor, HeaderNavigation, and AiExplanationPopover portal into the scope's portal root, not `<body>`. Nothing to configure.

- Do not pass `container={document.body}` inside a scope. An explicit `container` always wins, so the popup would leave the scope and lose its tokens.
- A popup inside another popup (Select in a Dialog) stays in its parent's portal, inside the scope.
- Render `Toast.Viewport` (and its provider) inside a `NesraScope`.
- Keep `transform`, `filter`, `perspective`, `contain`, `will-change: transform`, `isolation`, `z-index` with `position`, and `position: relative` with `overflow: hidden` off the `NesraScope` element itself. They confine the Dialog backdrop to the wrapper, clip menus, or trap stacking. Put them on an element inside the scope.

### Light and Dark

The scope follows `class="dark"` or `data-theme="dark"` on `<html>` or any ancestor, and on the `NesraScope` itself. A class on the wrapper wins over an ancestor (`<NesraScope className="light">` stays Light in a Dark app). `.dark` or `.light` inside the scope switches that subtree. Set the mode on `<html>` or on `NesraScope`. Do not write `dark:` variants.

## Limitations to expect

- Both releases are in the bundle until the older one is removed. The scoped stylesheet and 2.x utilities add about 240 kB raw (30 kB gzip) of CSS to the marketing client, measured on 23 Nov 2026.
- Fonts, `html`/`body` styles, and the reset stay with the older release. Load Manrope and Source Code Pro for 2.x.
- App CSS that sets `--nesra-*` on its own selectors (for example a table height override) also changes 2.x components inside the scope.
- `@keyframes` names are global. 2.x and 0.9 share `nesra-chart-dot-enter` with the same frames.

## Finish the rollout

When nothing renders the older release: replace `@nesra/ui-v2/...` imports with `@nesra/ui/...`, point `@nesra/ui` at 2.x and remove the alias, replace `@nesra/ui-v2/styles/scoped` with `@nesra/ui/styles` and drop the old stylesheet and `@source`, remove each `NesraScope` (keep its layout classes on a plain element), remove the per-folder import rules, and check every page in Light and Dark. No Nesra UI release is needed.
