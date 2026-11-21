---
name: nesra-ui
description: Build, migrate, or upgrade React interfaces with @nesra/ui (1.x or 2.x) through its public components and composition. Checks the installed version first and preserves Nesra UI's visual design instead of extending or restyling its components.
---

# Use Nesra UI

Apply this skill when the user wants an interface built with `@nesra/ui` or wants an existing interface migrated to it. Follow the requested scope while preserving Nesra UI's design system.

Nesra UI is meant to be composed, not extended or extensively modified. Build screens by combining public components, documented props, and ordinary layout markup. Do not replace component styles, colors, or typography to match a separate visual system. If a requested design requires those changes, explain the mismatch and seek direction before implementing that part.

## Lymo is the source of truth in 2.x

`@nesra/ui` 2.x is rebuilt from the Figma Lymo Design System. A component, type, variant, size, or color that Lymo does not have does not exist in 2.x: it was removed, with no deprecated alias. Do not recreate a removed component or variant from memory, from 1.x code, or from another library. If a screen needs something that is not in the installed 2.x package, explain the gap and ask before building a substitute.

## Check the installed version first

Before writing any import, prop, or token, find the `@nesra/ui` version the project uses: the `version` in `node_modules/@nesra/ui/package.json`, the resolved version in the lockfile (`pnpm-lock.yaml`, `package-lock.json`, or `yarn.lock`), or the range in the app's `package.json`. Then follow that major.

- **2.x (Design System 2.0).** Use the current docs (ui.nesra.ai, or stag-ui.nesra.ai for a pre-release review build), the 2.0 token names, and the 2.0 props and components.
- **1.x.** Use only the installed package's exports, type declarations, and `node_modules/@nesra/ui/dist/styles/tailwind.css`. The public docs describe 2.x, so do not apply their 2.0 token names, props, or components to a 1.x project. Do not upgrade the package unless the user asks.
- **Not installed yet.** Install the current major as the Installation page describes, then verify the public contract.

If the user asks to upgrade from 1.x to 2.x, follow [upgrade from 1.x to 2.x](references/upgrade-1-to-2.md).

## Find the supported API

- Check the installed package's `package.json` exports and TypeScript declarations before using imports, props, or compound parts. For a new installation, start with the current docs and verify the public contract after installing the package.
- Read the relevant Nesra UI documentation page for behavior and examples. The docs site publishes `/llms.txt`, individual Markdown pages such as `/components/button.md`, and `/llms-full.txt`. In the source repository, pages live under `apps/docs/src/content/docs/`.
- An upcoming page is a design placeholder, not a public API. If docs and the installed package differ, use the installed public contract and report the mismatch.
- For setup and token details, consult the installation and foundation pages. Do not copy changing component API details into this skill.

## Choose an approach

Prefer existing public Nesra components, then composition of public components and ordinary React markup. Do not select shadcn/ui or another component library by default. If a needed component or API still appears absent after checking the installed package and docs, explain the gap and ask the user before adding another library or building a substitute. Continue independent work while that decision is pending.

For headings and body copy, use the public `Text` component when the installed version provides it. Choose the HTML element with `as` to match the document structure, and choose the visual typography role with `variant` or the body `size` and `weight` props. A heading variant needs an explicit `as`; default body text renders a paragraph. Check the installed API and the Text documentation before using these props. Keep ordinary markup for layout and text elements that `Text` does not support.

Read [component and token decisions](references/component-and-token-decisions.md) when selecting components, composing a larger pattern, or styling it. In 2.x, read [Lymo components in 2.x](references/lymo-components.md) when a screen needs a Figma Lymo component that the decisions table does not name, or a Figma Select Type (Search, Tags, Avatar or Dot leading) or its focus states. In 2.x, read [Data grid in 2.x](references/data-grid.md) when a screen needs an editable, spreadsheet-style table: it is example code to copy, not a package export. In 2.x, read [Chart card variants](references/chart-card-variants.md) when a screen needs a chart inside a `ChartCard` (Area, stacked, grouped, or horizontal bars, and the variants not supported yet). In 2.x, read [Navigation current item](references/navigation-current-state.md) when a screen marks the current page in a `Sidebar`, `NavigationMenuItem`, or `HeaderNavigation` (the raised Figma `Current=True` state, Pressed, the Active + Focus stroke, their tokens, and the navigation parts without them). Read [migration workflow](references/migration.md) when replacing an existing UI library or custom component set. Read [upgrade from 1.x to 2.x](references/upgrade-1-to-2.md) when the user asks to move a project to `@nesra/ui` 2.x. For a new interface, use public imports and the documented stylesheet setup, preserve interaction and accessibility obligations, and run the host project's relevant checks.
