# Tabs

> Tabs switch between related views on the same screen. Underline tabs split a page into sections, pill tabs filter what's shown, and a pill group switches how content is shown.

- Group: Navigation
- Lifecycle: done
- Status: Figma synced
- Version: 1.12.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=4543-65938
- Source: `src/components/Tabs`
- Also called: Tab bar, chips, filter pills, pill group, segmented control, toggle group, view switcher

## Import

```tsx
import { Tabs, Tab } from './components/Tabs' // path relative to src/
```

## Overview

### Two components

**Tabs** (Figma "L3: Tabs group") is the bar: selection, horizontal scrolling on small screens, and arrow-key navigation. **Tab** (Figma "L3: base tab") is one item, in underline, pill or ribbon form, md or sm.

### Four appearances

- **underline** (Figma Flat tabs) — sections of a screen; at the top they go in the Actionbar's bottom slot.
- **pill** (Figma Pill tabs) — a row of filter chips in one style: primary, secondary or tertiary. Never mix styles in a row. The first chip lines up with the 16 page margin: in Figma, Pill tabs bring their own 16 side inset for edge-to-edge rows — if the container already has 16 padding, set the tabs' inset to 0 so it isn't doubled.
- **pill-group** (Figma Pill group) — 2–4 options in a shared track to switch how the same content is shown (Tree / List). Its pills are always tertiary: the selected one is a black fill, the rest blend into the track.
- **ribbon** (Figma Ribbon) — **special use only:** use it only when a design specifically asks for it; sections use underline. Folder tabs on a dark baseline: the selected tab is raised, with a dark outline whose sides curve into the line, and opens into the content under it. Tabs hug their labels, starting at the 16 margin; they act as tabs (switch sections).

### Tabs, filters or toggles

Only tabs that act as tabs fill the width. Ask what the row does:

- **Tabs** — switch to a different section or list (Holdings / Positions / Orders, Open / History). If every tab fits, they fill the row in equal widths; if not, they hug and the row scrolls.
- **Filters** — narrow what's shown (All / Stocks / F&O). Chips hug their labels, start at the 16 margin and scroll if needed.
- **Toggles** — show the same content another way (Tree / List, Chart / Table). The pill group hugs its options.

Defaults: Flat tabs act as tabs, Pill tabs as filters, Pill group as a toggle. Ribbon tabs (special use only) act as tabs but always hug their labels. A chip row that switches sections is set to act as tabs.

### Action pill

A pill that does something instead of picking a filter — sort the list, open a filter sheet — sits at the start of a pill row, before the filters. It looks like an unselected pill with an icon on the left (⇅ for sort) and its label says the current state: **Latest first**, **Highest P&L**. Tapping it changes the state; it never becomes selected.

`<Tab action iconLeft=… aria-label="Sort: Latest first. Switch to Oldest first" onClick=…>` — a plain button, outside the `<Tabs>` tablist.

### Same label, selected or not

Every tab label uses Label (650) — 14 for md, 12 for sm underline tabs, 12 / 10 for pills. Selection shows through the text color and the indicator bar (or the pill fill), never a bolder weight, so tabs never shift when the selection changes.

## Do / Don't

### Short, parallel labels

- ✅ **Do:** Two to four one-word sections of the same thing.
- ❌ **Don't:** Long labels that truncate, or tabs that act like buttons.

## Options (tree)

Tabs — Switch sections, filter or switch views

- **Appearance** — What it switches
  - `underline` — Sections of a screen. At the top: in the Actionbar bottom slot.
  - `pill` — A row of filter chips.
  - `pill-group` — 2–4 options in a track to switch views (Tree / List). Always tertiary.
  - `ribbon` — Special use only — when a design asks for it. Folder tabs on a dark baseline; tabs hug.
- **Size**
  - `md` — Default.
  - `sm` — Dense filter rows inside content.
- **Width**
  - `auto` — Default for real tabs (behavior="tabs"). Fills the row in equal widths when all tabs fit; hugs and scrolls when they don't.
  - `hug` — Default for filters and toggles. Always as wide as their labels.
  - `fill` — Tabs share the row; pill group pills become equal.
- **Pill options** — Chip tabs only
  - `emphasis: primary` — Default. Black fill when selected, light border when not.
  - `emphasis: secondary` — Dark outline when selected — inside cards and sheets.
  - `emphasis: tertiary` — Subtle fill, no border when unselected — quiet, dense rows.
  - `subLabel` — A second 8/10 line under the label.
  - `hideLabel` — Icon-only tab; the label stays as its name.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `TabItem[]` |  | Tabs: { value, label, iconLeft?, iconRight?, subLabel?, hideLabel? }. |
| `value` | `string` |  | Tabs: the selected item value. |
| `onChange` | `(value) => void` |  | Tabs: called on tap and on arrow / Home / End keys. |
| `appearance` | `'underline' \| 'pill' \| 'pill-group' \| 'ribbon'` | `'underline'` | Figma Tabs group Type: Flat tabs / Pill tabs / Pill group / Ribbon. A Tab alone takes underline \| pill \| ribbon (Figma base tab Style: Flat / Pill / Ribbon). |
| `emphasis` | `'primary' \| 'secondary' \| 'tertiary'` | `'primary'` | Figma base tab Type, appearance="pill" only (pill-group is always tertiary) — the style of the whole row. primary: selected black fill, unselected light border · secondary: selected dark outline, unselected light border · tertiary: selected black fill, unselected subtle fill with no border. |
| `size` | `'md' \| 'sm'` | `'md'` | Figma isSmall: underline 40 / 36, pill 32 / 24. |
| `behavior` | `'tabs' \| 'filter' \| 'toggle'` | `by appearance` | Tabs: what the row does. tabs switch sections (default for underline and ribbon) · filter narrows what's shown (default for pill) · toggle shows the same content another way (default for pill-group). Only tabs fill the width. |
| `width` | `'auto' \| 'hug' \| 'fill'` | `'auto' for tabs, 'hug' otherwise` | Tabs: auto = fill the row in equal widths if every tab fits, else hug and scroll · hug = tabs as wide as their labels (a pill group's track wraps them) · fill = tabs always stretch (a pill group goes full width with equal pills). |
| `aria-label` | `string` |  | Tabs: required name for the tab list. |
| `idPrefix` | `string` |  | Tabs: sets tab ids / aria-controls so panels can be linked. |
| `iconLeft / iconRight` | `ReactNode` |  | Figma icon slots, 16px, colored with the label. |
| `subLabel` | `string` |  | Figma 👁️ Sub label: a second 8/10 line under the label. Chip (pill) tabs only. |
| `hideLabel` | `boolean` | `false` | Figma 👁️ Label off: icon-only tab. Needs one icon; the label stays as its accessible name. |
| `selected` | `boolean` | `false` | Tab only, when composing tabs yourself. |

## Tokens used

- `content/primary · secondary · inverted`
- `surface/primary · secondary · inverted`
- `border/light · dark`
- `state-layer/*`
- `Label/08 · 10 · 12 · 14`
- `radius/12 · full`
- `size/24 · 32 · 40`
- `spacing/36`
- `size/tap-target`

## Recent changes

- **1.12.0** (2026-10-29) Pill tabs are raised: Pill (chip) tabs get the raised look, like buttons: the selected black pill the strong effect, the other pills the soft one (on white pills a faint black sheen, as on the Secondary button). It replaces the old elevation-low shadow on light pills. In a pill group only the selected pill is raised; the unselected pills stay flat on the track. Disabled pills stay flat. Underline and ribbon tabs are unchanged. Tab raised prop (strong · soft · none) to override the level; Tabs passes none for unselected pills in a pill group. Tab sets data-raised on pill tabs (components/Raised); the state layer and border feed --raise-layer / --raise-border.
- **1.11.1** (2026-10-21) Tab rows scroll sideways only: Every tab row (flat, pill, pill group, ribbon) now scrolls only sideways when its tabs overflow — a swipe can no longer move the row up and down.
- **1.11.0** (2026-10-14) Action pill: Action pill: a pill that does something instead of selecting — sort a list, open a filter sheet — set next to a pill row. It looks like an unselected pill (an icon fits on the left, e.g. ⇅ for sort) and its label says the current state ("Latest first"). action on Tab: a plain button (no tab role, not part of the selection); place it outside the <Tabs> tablist.
