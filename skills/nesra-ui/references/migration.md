# Migration workflow

Use this reference when replacing an existing component library or custom component set with `@nesra/ui`. Migrate only the scope the user requested.

## Inventory and mapping

Find component imports, local wrappers, styles, and uses of the old library in scope. Group by behavior rather than matching names alone. For each group, record the current component and interaction, its Nesra public replacement or composition, styling changes, and any unresolved gap. Check both the installed Nesra package and the relevant docs before calling something missing.

A similar appearance does not establish equivalent behavior. Pay particular attention to form state and validation, focus return, keyboard navigation, disabled and loading states, portals, and responsive behavior. Keep application-specific logic in the application while replacing the presentation and primitives that Nesra actually covers.

## Change and verify

- Replace usages in reviewable groups. Import only public Nesra entrypoints and apply the documented stylesheet setup once for the host app.
- Translate old color and role classes to documented Nesra semantic utilities. Preserve layout and interaction intent rather than carrying over selectors that target the old library's internals.
- Review representative default, interactive, error, disabled, and responsive states that exist in the migrated scope. Run the host project's relevant format, lint, typecheck, test, and build checks.
- Remove old dependencies, providers, wrappers, and CSS only after confirming they have no remaining consumers. Report intentional coexistence and confirmed gaps.

If the user requests a complete migration but a required behavior has no confirmed Nesra equivalent, compose public primitives or implement the application-specific behavior accessibly. Present a choice when meeting the requirement would materially change the requested behavior, design, or dependencies. Finish unrelated mapped groups while that choice is pending.
