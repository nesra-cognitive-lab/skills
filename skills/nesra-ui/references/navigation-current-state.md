# Navigation: current item (active state)

Source: Figma Lymo `_Nav item base` (`15236:3097`), `_Nav item dropdown base` (`15236:3170`), `_Nav collapsed item` (`16212:10410`), `_Nav button` (`15236:3880`). Docs: `components/sidebar/#current-item`, `components/navigation-menu-item/#states`, `components/header-navigation/#current-nav-item` (static Default / Hover / Active / Active + Focus previews, Light and Dark). Storybook: `Components/Sidebar` and `Components/Navigation Menu Item`, stories `Active States` and `Active States Dark`. Needs `@nesra/ui` with the raised Current state (design-system-nesra PR #276).

## Rules

- Mark the current page with the prop only. The component draws the Figma `Current=True` box and sets `aria-current="page"`:
  - `Sidebar.MenuButton active` (or `isActive`): expanded row, collapsed rail button, mobile drawer.
  - `Sidebar.MenuSubButton active` (or `isActive`): nested row.
  - `NavigationMenuItem active`.
  - `HeaderNavigation.NavItem current`.
- Do not rebuild the look with classes. Do not add `shadow-*`, `bg-*`, `border-*`, or a gradient to a current item, and never `shadow-[...]` or hex values.
- Do not copy the `className` overrides in the docs and Storybook state previews. They only pin Hover and Active + Focus so the states show without a pointer or keyboard.
- One current item per navigation. Do not mark a parent row and its child as current at the same time unless the product route is the parent.

## Current item spec

| State | Fill | Border (1px, inside) | Effect | Label and icon |
| --- | --- | --- | --- | --- |
| Default | transparent | transparent | none | Sidebar `text-tertiary` + `icon-quaternary`; Nav button `text-secondary` + `icon-quaternary` |
| Hover | `utility-brand-50` | transparent | none | Sidebar `text-tertiary`; Nav button `text-secondary_hover` |
| Focus | transparent | transparent | `shadow-nesra-focus-ring` | unchanged |
| Active (`Current=True`) | `utility-brand-50` → `utility-brand-100`, top to bottom | `border-brand` | `shadow-nesra-button-solid` (Figma `Shadows/button-solid`) | `text-brand-primary`, `icon-brand-primary` |
| Active + Hover | same as Active | `border-brand` | `shadow-nesra-button-solid` | same as Active |
| Active + Focus | same as Active | `border-brand_subtle` | `shadow-nesra-focus-ring` only | same as Active |

Active + Focus shows one ring: the focus ring replaces `button-solid`, as in the Figma Current=True + Focused variant.

`shadow-nesra-button-solid` layers (same as Figma, Light / Dark):

| Layer | CSS | Light | Dark |
| --- | --- | --- | --- |
| Highlight | `inset 0 2px 0.75px 0 var(--nesra-button-highlight)` | `#ffffff1a` | `#ffffff0f` |
| Bottom edge | `inset 0 -2px 0 0 var(--nesra-shadow-skeumorphic-inner-border)` | `#0000002e` | `#0000002e` |
| Drop shadow | `0 1px 3px -1px var(--nesra-shadow-sm_01)` | `#0000001a` | transparent |

The fill is Indigo 50 → 100 in Light and Indigo 950 → 900 in Dark. It is not `xs-skeuomorphic`: that token has an inset 1px ring and would make the border look 2px.

Sizes do not change when an item becomes current: Sidebar `compact` row 32px, 6 × 8px padding, `radius-md`; collapsed rail button 36px, `radius-lg`; Nav button 32px, 8 × 6px padding, `radius-md`.

## Examples

```tsx
import { Sidebar } from "@nesra/ui/sidebar";
import { House, BarChart3 } from "lucide-react";

<Sidebar.Menu>
  <Sidebar.MenuItem>
    <Sidebar.MenuButton active render={<Link href="/" />} icon={<House />}>
      Home
    </Sidebar.MenuButton>
  </Sidebar.MenuItem>
  <Sidebar.MenuItem>
    <Sidebar.MenuButton render={<Link href="/analytics" />} icon={<BarChart3 />}>
      Analytics
    </Sidebar.MenuButton>
    <Sidebar.MenuSub>
      <Sidebar.MenuSubItem>
        <Sidebar.MenuSubButton isActive={pathname === "/analytics/revenue"} render={<Link href="/analytics/revenue" />}>
          Revenue
        </Sidebar.MenuSubButton>
      </Sidebar.MenuSubItem>
    </Sidebar.MenuSub>
  </Sidebar.MenuItem>
</Sidebar.Menu>
```

```tsx
import { NavigationMenuItem } from "@nesra/ui/navigation-menu-item";

<NavigationMenuItem active render={<Link href="/overview" />}>Overview</NavigationMenuItem>
```

```tsx
import { HeaderNavigation } from "@nesra/ui/header-navigation";

<HeaderNavigation.Nav aria-label="Main">
  <HeaderNavigation.NavItem current render={<Link href="/home" />}>Home</HeaderNavigation.NavItem>
  <HeaderNavigation.NavItem render={<Link href="/projects" />}>Projects</HeaderNavigation.NavItem>
</HeaderNavigation.Nav>
```

## Navigation parts without this effect

These have no raised Current state in Figma. Do not add one.

| Part | Figma | Current or selected look |
| --- | --- | --- |
| `HeaderNavigation.NavButton` (icon only) | `_Nav button` Icon only=True | No `current` prop. Hover `utility-brand-50` |
| `HeaderNavigation.MenuButton` | `_Nav menu button` (`15236:3861`) | No Current state in Figma |
| `Sidebar.AccountMenu.Item`, `.AccountItem` | `_Nav account card menu item` (`15236:3929`) | Current = flat `utility-brand-50` fill |
| `Sidebar.PlatformMenu.Item` | `_Platform menu item` (`16158:10824`) | Selected = `text-brand-primary` label and a check circle |
| `AppRail` item | `_App rail item` (`15386:3094`) | Active = 2px `icon-brand-primary` tile stroke |
| `Sidebar.AccountMenu` card, `Sidebar.FeaturedCard` | `_Nav account card`, `_Nav featured card` | No Current state |

Figma also draws a Pressed state for `_Nav item base` and `_Nav button` (`utility-brand-100`, `border-brand`, `shadow-nesra-button-solid-pressed`). The package does not apply it yet; do not add it per screen.
