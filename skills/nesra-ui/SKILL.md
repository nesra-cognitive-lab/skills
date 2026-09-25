---
name: nesra-ui
description: Build or migrate React interfaces with @nesra/ui. Use for Nesra component selection, public API checks, Tailwind setup, semantic styling, and UI library migration.
---

# Use Nesra UI

Apply this skill when the user wants an interface built with `@nesra/ui` or wants an existing interface migrated to it. Follow the project's chosen design and scope.

## Find the supported API

- Identify the project's installed version. Use its `package.json` exports and TypeScript declarations to confirm imports, props, and compound parts. Do not assume the latest release is installed or upgrade it unless the task calls for an upgrade.
- Read the relevant documentation page for behavior and examples when available. The docs publish `/llms.txt`, individual Markdown pages such as `/components/button.md`, and `/llms-full.txt`; source pages are under `apps/docs/src/content/docs/`. If those docs are inaccessible, use the installed package's public declarations and README, and verify behavior in the application.
- Treat an upcoming docs page as a design placeholder. If docs and the installed package differ, follow the installed public contract and mention the mismatch when it affects the result. Do not copy changing component API details into this skill.

## Choose an approach

Prefer existing public Nesra components, then composition of public components and ordinary React markup. Keep application-specific layout and behavior in the application. Do not select another component library by default. If the public API cannot meet a required interaction, choose an accessible local implementation within the requested scope; ask only when the available options would materially change the requested design, behavior, or dependencies.

For headings and body copy, use the public `Text` component when the installed version provides it. Choose the HTML element with `as` to match the document structure, and choose the visual typography role with `variant` or the body `size` and `weight` props. A heading variant needs an explicit `as`; default body text renders a paragraph. Check the installed API and the Text documentation before using these props. Keep ordinary markup for layout and text elements that `Text` does not support.

Read [component and token decisions](references/component-and-token-decisions.md) when selecting components, composing a larger pattern, or styling it. Read [migration workflow](references/migration.md) when replacing an existing UI library or custom component set. For a new installation, follow the package README's Tailwind v4 setup: source the package's `dist/**/*.js`, import `@nesra/ui/styles`, and import `tailwindcss` in the application's global stylesheet. Adjust the relative `node_modules` path for the host project. Preserve interaction and accessibility obligations, and run the host project's relevant checks.
