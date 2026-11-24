# Dropdown menu: Figma types, parts, and Sign out

Source: Figma Lymo Dropdowns page (`18:0`), frame `Dropdown menu` (`15185:2576`) with its Dark preview (`16890:8751`), and the parts `_Dropdown menu list item` (`15180:1547`), `_Dropdown menu item inset icon` (`15180:1500`), `_Dropdown menu header` (`15181:1517`), `_Dropdown menu footer` (`15181:1530`), and `_Dropdown account list item` (`15181:1558`). Docs: `components/dropdown/`, one section per Figma type in Figma order. Storybook: `Components/Dropdown Menu`, one open story per type plus its `Dark` story (for example `Button advanced` and `Button advanced Dark`), and `Trigger matrix`. `Trigger variant`, `RadioItem indicator`, `SubTrigger checked`, and `AccountTrigger status` need `@nesra/ui` with design-system-nesra PR #298.

## Rules

- A dropdown menu runs actions. Use `Select` or `Combobox` when a trigger picks a form value.
- Use the Figma trigger: `DropdownMenu.Trigger variant="button"` (34px Controls button), `"link"` (brand text), or `"icon"` (26px, needs `aria-label`), or `DropdownMenu.AccountTrigger` for the account types. Pass only the label: the chevron is part of the variant. Do not wrap a `Button` in `render` to imitate a Figma trigger.
- `DropdownMenu.Content` is 240px. Set the Figma width in `className` for the other types: `w-54` (216px) for Button simple, Icon simple, Integrations; `w-50` (200px) for Account card xs and every submenu (`SubContent`); `w-40` (160px) for Button link; `w-62` (248px) for Account breadcrumb. Align it to the trigger's right edge with `align="end"`, as Figma does.
- Inset icons are direct children of the item: a lucide icon (14px `icon-quaternary`), an `Avatar className="size-4" size="xs"`, or `IntegrationIcon className="size-4" grayscale`. Key hints go last in `DropdownMenu.Shortcut`.
- Checked toggles: `CheckboxItem` (Check inset icon) or `CheckboxItem indicator="checkbox"` (Checkbox inset icon). One choice: `RadioGroup` with `RadioItem` (Check) or `RadioItem indicator="dot"` (Figma Dot, 6px `icon-success-secondary`).
- A row that is checked and opens a submenu (Search advanced team): `SubTrigger checked`. It stays a `menuitem` and adds `checkedLabel` (default `"selected"`) to its name.
- The current account in a "Switch account" group is the Figma selected row: `className="bg-nesra-bg-secondary_hover text-nesra-text-primary"` on that `Item`. The current role in Button link: `className="data-[checked]:bg-nesra-bg-secondary_hover data-[checked]:text-nesra-text-primary"` on the `RadioItem`. A checked `AccountItem` draws that surface itself.
- Long lists scroll with the Figma menu scrollbar; set the Figma height with `className="max-h-73"` (292px, Search simple). `DropdownMenu.Search` stays at the top and `DropdownMenu.Footer` at the bottom. The app owns the search query and filters the items.
- Use the Figma content in mocks: Olivia Rhye `olivia@lymo.design`, Sienna Hewitt, Caitlyn King, photos (not initials) for people.
- A real account without a photo shows initials, not the person icon. `Avatar` never derives initials from `name`: pass `initials` (for example `"OR"`, first letters of the first two words, decided by the app) to `DropdownMenu.AccountTrigger` and to the header `Avatar`. Without `initials` both show Avatar's default person icon. `AccountTrigger initials` needs `@nesra/ui` with design-system-nesra PR #304; `variant="button"` has no avatar and ignores it. `AccountItem` has no `initials` yet.
- An entry Figma shows but the app cannot run yet (for example Dark mode before the app supports it): keep it in the Figma position with `disabled` on its `Item` or `SubTrigger`. The 2.x item draws Figma Disabled (50% opacity, no hover) and sets `aria-disabled`; a disabled `SubTrigger` needs no `SubContent`. Leave out entries for features the app does not have at all.

## Sign out: item or footer button

| Pattern | Figma types | When |
| --- | --- | --- |
| `<DropdownMenu.Footer variant="button"><DropdownMenu.FooterButton>Sign out</DropdownMenu.FooterButton></DropdownMenu.Footer>` | Account button, Account avatar | The menu ends with Sign out and has no other footer. |
| `<DropdownMenu.Item><LogOut aria-hidden="true" />Sign out</DropdownMenu.Item>` after a `Separator` | Account card xs, sm, md | The menu has the text footer (`© Nesra AI`, version) or is the compact 200px menu. |

Never use both in one menu. Sign out is never `variant="destructive"` and never red: Figma draws it in the regular item or button colors. Figma has no destructive item at all, so the Delete items (Button simple, Button link) use the regular colors too.

## Figma type → parts

| Figma type (node) | Docs section | Parts |
| --- | --- | --- |
| Button simple (`15182:1523`) | Button simple | `Trigger variant="button"`, `Content className="w-54"`, `Item`, `Shortcut`, `Separator`, `Sub` |
| Button advanced (`15182:1615`) | Button advanced | `Trigger variant="button"`, `Item` with icon, `CheckboxItem`, `RadioItem indicator="dot"`, nested `Sub` |
| Button link (`15182:1802`) | Button link | `Trigger variant="link"`, `Content className="w-40"`, `RadioItem`, `Item` with `Trash2` |
| Icon simple (`15182:1861`) | Icon simple | `Trigger variant="icon"` + `EllipsisVertical`, Button simple parts |
| Icon advanced (`15182:1951`) | Icon advanced | `Trigger variant="icon"`, Button advanced parts |
| Search simple (`15183:1892`) | Search simple | `Search`, `CheckboxItem indicator="checkbox"`, `Content className="max-h-73"` |
| Search advanced (`15183:2031`) | Search advanced | `Search`, `SubTrigger checked`, `Item` with `Avatar`, `CheckboxItem indicator="checkbox"`, `Footer variant="button"` |
| Integrations (`15183:2200`) | Integrations | `Trigger variant="button"`, `Content className="w-54"`, `Item` with `IntegrationIcon grayscale` |
| Account button (`15183:2382`) | Account button | `AccountTrigger variant="button" label="Account"`, profile items, "Switch account" `Label`, Sign out footer button |
| Account avatar (`15184:2242`) | Account avatar | `AccountTrigger variant="avatar" src`, `Header avatar={<Avatar border size="sm" status="online" src />}`, Sign out footer button |
| Account card xs (`15184:2355`) | Account card xs | `AccountTrigger size="xs" src`, `Content className="w-50"`, Sign out item |
| Account card sm (`15184:2463`) | Account card sm | `AccountTrigger size="sm" status="online" src`, `Header title="PRO account"`, Sign out item, `Footer` text |
| Account card md (`15184:2608`) | Account card md | `AccountTrigger size="md" email status="online" src`, `Header`, "Company" `RadioGroup`, Sign out item, `Footer` text |
| Account breadcrumb (`15184:2742`) | Account breadcrumb | `AccountTrigger variant="breadcrumb" src`, `Content className="w-62"`, `AccountItem` (checkbox at the end) |

## Code

Actions (Button advanced):

```tsx
<DropdownMenu>
  <DropdownMenu.Trigger variant="button">Actions</DropdownMenu.Trigger>
  <DropdownMenu.Content align="end">
    <DropdownMenu.Item>
      <RefreshCw aria-hidden="true" />
      Reload
      <DropdownMenu.Shortcut>⌘R</DropdownMenu.Shortcut>
    </DropdownMenu.Item>
    <DropdownMenu.Separator />
    <DropdownMenu.CheckboxItem checked={showBookmarks} onCheckedChange={setShowBookmarks}>
      Show bookmarks
    </DropdownMenu.CheckboxItem>
    <DropdownMenu.Separator />
    <DropdownMenu.RadioGroup onValueChange={setPerson} value={person}>
      <DropdownMenu.RadioItem indicator="dot" value="olivia">Olivia Rhye</DropdownMenu.RadioItem>
      <DropdownMenu.RadioItem indicator="dot" value="sienna">Sienna Hewitt</DropdownMenu.RadioItem>
    </DropdownMenu.RadioGroup>
    <DropdownMenu.Separator />
    <DropdownMenu.Sub>
      <DropdownMenu.SubTrigger>
        <Wrench aria-hidden="true" />
        More tools
      </DropdownMenu.SubTrigger>
      <DropdownMenu.SubContent className="w-50">
        <DropdownMenu.Item>
          <Scissors aria-hidden="true" />
          Cut
          <DropdownMenu.Shortcut>⌘X</DropdownMenu.Shortcut>
        </DropdownMenu.Item>
      </DropdownMenu.SubContent>
    </DropdownMenu.Sub>
  </DropdownMenu.Content>
</DropdownMenu>
```

Account with Switch account and the Sign out footer button (Account button):

```tsx
<DropdownMenu>
  <DropdownMenu.AccountTrigger label="Account" name="Olivia Rhye" variant="button" />
  <DropdownMenu.Content align="end">
    <DropdownMenu.Item>
      <Settings aria-hidden="true" />
      Settings
      <DropdownMenu.Shortcut>⌘S</DropdownMenu.Shortcut>
    </DropdownMenu.Item>
    <DropdownMenu.Separator />
    <DropdownMenu.Group>
      <DropdownMenu.Label>Switch account</DropdownMenu.Label>
      <DropdownMenu.Item className="bg-nesra-bg-secondary_hover text-nesra-text-primary">
        <Avatar className="size-4" name="Olivia Rhye" size="xs" src={olivia.photo} />
        Olivia Rhye
      </DropdownMenu.Item>
      <DropdownMenu.Item>
        <Plus aria-hidden="true" />
        Add account
      </DropdownMenu.Item>
    </DropdownMenu.Group>
    <DropdownMenu.Footer variant="button">
      <DropdownMenu.FooterButton onClick={signOut}>Sign out</DropdownMenu.FooterButton>
    </DropdownMenu.Footer>
  </DropdownMenu.Content>
</DropdownMenu>
```

Avatar trigger with the profile header (Account avatar). For an account without a photo, replace `src` with `initials` on both avatars, for example `initials="OR"`:

```tsx
<DropdownMenu>
  <DropdownMenu.AccountTrigger name="Olivia Rhye" src={olivia.photo} variant="avatar" />
  <DropdownMenu.Content align="end">
    <DropdownMenu.Header
      avatar={<Avatar border name="Olivia Rhye" size="sm" src={olivia.photo} status="online" />}
      description="olivia@lymo.design"
      title="Olivia Rhye"
    />
    <DropdownMenu.Separator />
    <DropdownMenu.Item>
      <UserRound aria-hidden="true" />
      View profile
      <DropdownMenu.Shortcut>⌘P</DropdownMenu.Shortcut>
    </DropdownMenu.Item>
    <DropdownMenu.Footer variant="button">
      <DropdownMenu.FooterButton onClick={signOut}>Sign out</DropdownMenu.FooterButton>
    </DropdownMenu.Footer>
  </DropdownMenu.Content>
</DropdownMenu>
```

Manage access with search and checkboxes (Search simple):

```tsx
<DropdownMenu>
  <DropdownMenu.Trigger variant="button">Manage access</DropdownMenu.Trigger>
  <DropdownMenu.Content align="end" className="max-h-73">
    <DropdownMenu.Search onValueChange={setQuery} placeholder="Search people" value={query} />
    {matches.map((person) => (
      <DropdownMenu.CheckboxItem
        checked={checked.includes(person)}
        closeOnClick={false}
        indicator="checkbox"
        key={person}
        onCheckedChange={(next) => toggle(person, next)}
      >
        {person}
      </DropdownMenu.CheckboxItem>
    ))}
  </DropdownMenu.Content>
</DropdownMenu>
```

Account list (Account breadcrumb):

```tsx
<DropdownMenu>
  <DropdownMenu.AccountTrigger name={account.name} src={account.photo} variant="breadcrumb" />
  <DropdownMenu.Content align="end" className="w-62">
    {accounts.map((item) => (
      <DropdownMenu.AccountItem
        checked={item.id === current}
        email={item.email}
        key={item.id}
        name={item.name}
        onCheckedChange={() => setCurrent(item.id)}
        src={item.photo}
      />
    ))}
  </DropdownMenu.Content>
</DropdownMenu>
```

## Not in Figma

The docs keep behavior sections with no Figma frame: Basic, Width, Radio items and checkbox items (Table menus), Controlled open state, Keep the menu open (`closeOnClick={false}`), Placement, Submenu, Multiple, Destructive, Item content, Notifications (`NotificationItem`), Triggers, and Header and footer. Use them for behavior and API, not as visual references.

## Known issue

`DropdownMenu.Footer` sits 4px higher than in Figma (found 24 Nov 2026, not fixed yet): it is `sticky bottom-0` with a negative bottom margin, so the menu's 4px bottom padding pushes it up. It covers the bottom 4px of the last item above it and leaves a 4px strip of the menu surface below the footer. Do not patch it with classes in an app; it will be fixed in `@nesra/ui`.
