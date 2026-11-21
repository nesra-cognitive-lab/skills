# Navigation: current item (active state)

Source: Figma Lymo `_Nav item base` (`15236:3097`), `_Nav item dropdown base` (`15236:3170`), `_Nav collapsed item` (`16212:10410`, a `_Nav button` Icon only=True inside), `_Nav button` (`15236:3880`). Docs: `components/sidebar/#current-item`, `components/navigation-menu-item/#states`, `components/header-navigation/#current-nav-item` and `#current-icon-only-nav-button` (static Default / Hover / Pressed / Active / Active + Hover / Active + Pressed / Active + Focus previews, Light and Dark). Storybook: `Components/Sidebar` and `Components/Navigation Menu Item` stories `Active States` and `Active States Dark`; `Components/Header Navigation` stories `Nav Button Current` and `Nav Button Current Dark`. Needs `@nesra/ui` with the raised Current state (design-system-nesra PR #276) and Pressed, the Figma focus stroke, and `NavButton current` (PR #281).

## Rules

- The app decides which item is current (a router or state). The components do not change it. Pass the prop from the route or state and update it when the user picks another item, so a click or Tab + Enter moves the current item.
- Mark the current page with the prop only. The component draws the Figma `Current=True` box and sets `aria-current="page"`:
  - `Sidebar.MenuButton active` (or `isActive`): expanded row, collapsed rail button, mobile drawer.
  - `Sidebar.MenuSubButton active` (or `isActive`): nested row.
  - `NavigationMenuItem active`.
  - `HeaderNavigation.NavItem current`.
  - `HeaderNavigation.NavButton current` (icon only). An explicit `aria-current` wins.
- Pressed, Hover, and Focus come from the pointer and keyboard. Do not add them per screen.
- Do not rebuild the look with classes. Do not add `shadow-*`, `bg-*`, `border-*`, `outline-*`, or a gradient to a nav item, and never `shadow-[...]` or hex values.
- Do not copy the `className` overrides in the docs and Storybook state previews. They only pin Hover, Pressed, Active + Pressed, and Active + Focus so the states show without a pointer or keyboard.
- One current item per navigation. Do not mark a parent row and its child as current at the same time unless the product route is the parent (Figma `_Nav item dropdown base` Open=True Current=True, as in the Header navigation mobile drawer).

## State spec

| State | Fill | Stroke | Effect | Label and icon |
| --- | --- | --- | --- | --- |
| Default | transparent | none | none | Sidebar `text-tertiary` + `icon-quaternary` (collapsed `icon-tertiary`); Nav button `text-secondary` + `icon-quaternary`; NavButton icon `icon-tertiary` |
| Hover | `utility-brand-50` | none | none | Sidebar `text-tertiary`; Nav button `text-secondary_hover`; NavButton `icon-tertiary_hover` |
| Pressed (Current True or False) | `utility-brand-100` | 1px `border-brand`, inside | `shadow-nesra-button-solid-pressed` (Figma `Shadows/button-solid-pressed`) | `text-brand-primary` (Semibold on `compact` sidebar rows), `utility-brand-600` icons |
| Focus | transparent | none | `shadow-nesra-focus-ring` | unchanged |
| Active (`Current=True`) | `utility-brand-50` → `utility-brand-100`, top to bottom | 1px `border-brand`, inside | `shadow-nesra-button-solid` (Figma `Shadows/button-solid`) | `text-brand-primary`, `utility-brand-600` icons |
| Active + Hover | same as Active | same as Active | `shadow-nesra-button-solid` | same as Active |
| Active + Pressed | `utility-brand-100`, no gradient | 1px `border-brand`, inside | `shadow-nesra-button-solid-pressed` | same as Active |
| Active + Focus | same as Active | Expanded and nested sidebar rows (`_Nav item base`): 2px `border-brand_subtle`, **outside**. Collapsed rail, `NavigationMenuItem`, `NavButton` (`_Nav button`): 1px `border-brand_subtle`, inside | `shadow-nesra-focus-ring` only | same as Active |

Active + Focus shows one ring: the focus ring replaces `button-solid`. On expanded and nested sidebar rows the code draws the 2px outside stroke as an outline (`outline-2 outline-offset-0 outline-nesra-border-brand_subtle`) over the ring's 2px `bg-primary` gap, so the row reads 2px `border-brand_subtle` then 2px `focus-ring`, as in Figma.

A menu trigger whose popup is open (`data-popup-open`) keeps its open style while pressed.

Effect layers (same as Figma, Light / Dark):

| Token | CSS | Light | Dark |
| --- | --- | --- | --- |
| `shadow-nesra-button-solid` highlight | `inset 0 2px 0.75px 0 var(--nesra-button-highlight)` | `#ffffff1a` | `#ffffff0f` |
| `shadow-nesra-button-solid` bottom edge | `inset 0 -2px 0 0 var(--nesra-shadow-skeumorphic-inner-border)` | `#0000002e` | `#0000002e` |
| `shadow-nesra-button-solid` drop shadow | `0 1px 3px -1px var(--nesra-shadow-sm_01)` | `#0000001a` | transparent |
| `shadow-nesra-button-solid-pressed` | `inset 0 1px 2px 0 var(--nesra-shadow-skeumorphic-inner-border)` | `#0000002e` | `#0000002e` |
| `shadow-nesra-focus-ring` | `0 0 0 2px var(--nesra-bg-primary), 0 0 0 4px var(--nesra-focus-ring)` | white gap, `#6366f1` | `#0a0a0a` gap, `#6366f1` |

The Active fill is Indigo 50 → 100 in Light and Indigo 950 → 900 in Dark. The Pressed fill is Indigo 100 / Indigo 900. `utility-brand-600` is Indigo 600 / Indigo 400. Active is not `xs-skeuomorphic`: that token has an inset 1px ring and would make the border look 2px.

Sizes do not change with state: Sidebar `compact` row 32px, 6 × 8px padding, `radius-md`; collapsed rail button 36px, `radius-lg`; Nav button 32px, 8 × 6px padding, `radius-md`; `NavButton` 36px, `radius-lg`, 20px icon centered.

## Examples

```tsx
import { Sidebar } from "@nesra/ui/sidebar";
import { House, BarChart3 } from "lucide-react";

// With a router: const current = pathname. Without one, keep it in state.
const [current, setCurrent] = useState("/");

<Sidebar.Menu>
  <Sidebar.MenuItem>
    <Sidebar.MenuButton
      active={current === "/"}
      onClick={() => setCurrent("/")}
      render={<Link href="/" />}
      icon={<House />}
    >
      Home
    </Sidebar.MenuButton>
  </Sidebar.MenuItem>
  <Sidebar.MenuItem>
    <Sidebar.MenuButton render={<Link href="/analytics" />} icon={<BarChart3 />}>
      Analytics
    </Sidebar.MenuButton>
    <Sidebar.MenuSub>
      <Sidebar.MenuSubItem>
        <Sidebar.MenuSubButton
          isActive={current === "/analytics/revenue"}
          onClick={() => setCurrent("/analytics/revenue")}
          render={<Link href="/analytics/revenue" />}
        >
          Revenue
        </Sidebar.MenuSubButton>
      </Sidebar.MenuSubItem>
    </Sidebar.MenuSub>
  </Sidebar.MenuItem>
</Sidebar.Menu>
```

```tsx
import { NavigationMenuItem } from "@nesra/ui/navigation-menu-item";

<NavigationMenuItem active={pathname === "/overview"} render={<Link href="/overview" />}>
  Overview
</NavigationMenuItem>
```

```tsx
import { HeaderNavigation } from "@nesra/ui/header-navigation";
import { Bell, LayoutGrid } from "lucide-react";

<HeaderNavigation.Nav aria-label="Main">
  <HeaderNavigation.NavItem current={pathname === "/home"} render={<Link href="/home" />}>
    Home
  </HeaderNavigation.NavItem>
  <HeaderNavigation.NavItem current={pathname === "/projects"} render={<Link href="/projects" />}>
    Projects
  </HeaderNavigation.NavItem>
</HeaderNavigation.Nav>
<HeaderNavigation.Actions>
  {/* Icon-only destination: current draws the same raised box with a utility-brand-600 icon. */}
  <HeaderNavigation.NavButton
    current={pathname === "/apps"}
    icon={<LayoutGrid />}
    label="Apps"
    render={<Link href="/apps" />}
  />
  <HeaderNavigation.NavButton count={2} icon={<Bell />} label="Notifications" />
</HeaderNavigation.Actions>
```

## Navigation parts without this effect

These have no raised Current state in Figma. Do not add one.

| Part | Figma | Current or selected look |
| --- | --- | --- |
| `HeaderNavigation.MenuButton` | `_Nav menu button` (`15236:3861`) | No Current state in Figma |
| `Sidebar.AccountMenu.Item`, `.AccountItem` | `_Nav account card menu item` (`15236:3929`) | Current = flat `utility-brand-50` fill |
| `Sidebar.PlatformMenu.Item` | `_Platform menu item` (`16158:10824`) | Selected = `text-brand-primary` label and a check circle |
| `AppRail` item | `_App rail item` (`15386:3094`) | Active = 2px `icon-brand-primary` tile stroke |
| `Sidebar.AccountMenu` card, `Sidebar.FeaturedCard` | `_Nav account card`, `_Nav featured card` | No Current state |

