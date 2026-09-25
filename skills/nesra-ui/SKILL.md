---
name: nesra-ui
description: Build or migrate React interfaces in projects that use or are adopting @nesra/ui. Guides component discovery, composition, semantic styling, and migration decisions without duplicating component APIs.
---

# Use Nesra UI

Apply this skill when the user wants an interface built with `@nesra/ui` or wants an existing interface migrated to it. Follow the project's chosen design and scope.

## Find the supported API

- If `@nesra/ui` is installed, identify its version and check its `package.json` exports and TypeScript declarations before using imports, props, or compound parts. For a new installation, start with the current docs and verify the public contract after installing the package.
- Read the relevant Nesra UI documentation page for behavior and examples. The docs site publishes `/llms.txt`, individual Markdown pages such as `/components/button.md`, and `/llms-full.txt`. In the source repository, pages live under `apps/docs/src/content/docs/`.
- An upcoming page is a design placeholder, not a public API. If docs and the installed package differ, use the installed public contract and report the mismatch.
- For setup and token details, consult the installation and foundation pages. Do not copy changing component API details into this skill.

## Choose an approach

Prefer existing public Nesra components, then composition of public components and ordinary React markup. Do not select shadcn/ui or another component library by default. If a needed component or API still appears absent after checking the installed package and docs, explain the gap and ask the user before adding another library or building a substitute. Continue independent work while that decision is pending.

For headings and body copy, use the public `Text` component when the installed version provides it. Choose the HTML element with `as` to match the document structure, and choose the visual typography role with `variant` or the body `size` and `weight` props. A heading variant needs an explicit `as`; default body text renders a paragraph. Check the installed API and the Text documentation before using these props. Keep ordinary markup for layout and text elements that `Text` does not support.

Read [component and token decisions](references/component-and-token-decisions.md) when selecting components, composing a larger pattern, or styling it. Read [migration workflow](references/migration.md) when replacing an existing UI library or custom component set. For a new interface, use public imports and the documented stylesheet setup, preserve interaction and accessibility obligations, and run the host project's relevant checks.
