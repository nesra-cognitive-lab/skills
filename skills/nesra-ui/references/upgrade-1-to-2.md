# Upgrade from 1.x to 2.x

Use this reference only when the user asks to move a project from `@nesra/ui` 1.x to 2.x. The docs page [Upgrading to 2.0](https://ui.nesra.ai/upgrading-to-2-0/) (`/upgrading-to-2-0.md` for agents, or `apps/docs/src/content/docs/upgrading-to-2-0.mdx` in the source repository) holds the complete token and prop tables. Read it instead of guessing a mapping. This reference is the workflow around it.

## Plan

1. Confirm the installed version is 1.x and that the user wants the upgrade now. Some apps stay on 1.x on purpose.
2. Install 2.x (`pnpm add @nesra/ui@2`, or the project's package manager). Keep the stylesheet setup from the Installation page. Add Source Code Pro (400, 500) next to Inter.
3. Inventory before changing anything. Run the search commands from the guide's "Search commands" section and record the hits by group: colors, spacing, radius, text styles, shadows, and the component props below.

## The five high-impact changes

Check these first, because TypeScript catches only some of them:

- **Button sizes** keep their names but change height (`sm` 34px, `md` 38px, `lg` 42px, new `xs` and `xl`). Review layouts tuned to the old heights. Remove `size` props on icons inside Button.
- **Avatar size names** shifted one step without a type error: old `sm` → `xs`, `md` → `sm`, `lg` → `md`. Update every `<Avatar size=` and check `Avatar.Group`.
- **Badge colors** `neutral` → `gray` and `info` → `blue`. TypeScript reports these.
- **Tooltip is light.** Replace inverse text and borders in custom tooltip content with `text-primary` and `border-secondary`. Move rich content to `Popover`.
- **Input `size`** is now `"sm" | "md" | "lg"`. Replace a numeric `size` with a CSS width.

## Replace tokens

- Move each 1.0 token to the 2.0 token in the guide's tables. Do not search and replace by name alone: names that exist in both versions can change value (the radius scale moved one step, brand roles are indigo, `alpha-white-10` and `alpha-black-50` flip in Dark).
- Spacing moves by pixel value. A 1.0 size with no 2.0 token uses `control-height-*` for controls or the Tailwind scale.
- Add a text color next to every 2.0 text style class.
- If the project used the 2.0 pre-release, replace `nesra-v2-` with `nesra-` after the 1.0 names are gone.
- Work in reviewable groups, one token group or component family at a time.

## Verify

- Run the project's typecheck, lint, tests, and build.
- Rerun the search commands. Every remaining hit needs a 2.0 name or a recorded decision.
- Review key screens in Light and Dark: lists, forms with errors, dialogs, toasts, tables with selection, and charts.
- Check keyboard behavior: Tab order, visible focus rings, arrow keys in Tabs and ButtonGroup, focus trapping and return in Dialog and Sheet, and Escape.

## When something has no 2.0 counterpart

The guide marks some tokens "pending design" and some APIs deprecated. Report each one to the user with its location and the guide's suggestion. Do not restyle component internals, copy 1.0 values into custom CSS, or add a substitute component to recreate the 1.x look.
