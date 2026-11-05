# Side nav

> A side nav is a column of square tiles — icon over a short label — that switches the panel beside it, like a tools sheet with Range, Indicators, Drawings and Settings.

- Group: Navigation
- Lifecycle: wip
- Version: 1.0.0
- Source: `src/components/SideNav`
- Also called: Vertical tabs, side tabs, tool rail, navigation rail, sidebar, category list, tile nav

## Import

```tsx
import { SideNav } from './components/SideNav' // path relative to src/
```

## Usage rules (USAGE.md)

`import { SideNav } from './components/SideNav'` · Figma: not yet (code first, from the Lemonn chart-tools sheet) ·
Docs: `#/side-nav`

A vertical column of square tiles — icon over a short label — that **switches the panel beside it**. It's tabs turned
on their side for a panel too narrow for a top tab row and with more groups than fit across: a tools sheet (Range ·
Indicators · Drawings · Templates · Settings) or a settings area with a few groups.

---

### 1. When to use it

- **3–7 groups** of one tool or settings area, shown **side by side with their content** (the nav on the left, the
  panel on the right), usually inside a `BottomSheet` or a wide panel.

Don't use it for:
- **2–5 sections across the top of a screen or sheet** → `Tabs` (underline; pills for filters).
- **Moving between screens** → `BottomNavbar` / list rows that push a screen.
- **Choosing a value** (one of a few options) → radio rows, `ListCell` `selected`, `Select`.
- **More than ~7 groups** → a list that opens each group.

### 2. Layout

- The column is **96 wide** (tiles 80 with 8 either side) and **16 from the top**; tiles are square and fill the
  column's width, **10 apart**. A 1px `border/light` hairline separates the column from the panel.
- The column scrolls on its own when the tiles don't fit; the panel scrolls separately.
- The panel beside it starts with the selected group's name (Label/16) — the same word as the tile.

### 3. States

| State | Look |
|---|---|
| Unselected | no fill, `border/light` outline, icon + label `content/secondary` |
| Selected | filled — `surface/secondary` (`selectedTone="neutral"`, default) or the discover tint (`selectedTone="discover"`); no outline; `content/primary` |
| Hover (pointer only) | state layer on unselected tiles |
| Pressed | press scale (0.98) |
| Focus | 2 `border/dark` ring |

- Exactly **one** tile is selected — the nav always shows a panel.
- `selectedTone`: keep **neutral** unless the product's selection colour for tools is blue (Lemonn's chart tools use
  `discover`). Use one tone per product, not per screen.

### 4. Content

- **Label:** one word, two at most (*Indicators*, *Drawings*). It truncates with … past the tile — keep it short
  rather than relying on that.
- **Icon:** one 24 icon that names the group; outlined or filled, but the same style across the column.
- **Flag** (`flag="NEW"`): for a group that's new — one per nav, removed after a release or two. `flagTone="zing"`
  only for something new in Zing; otherwise the default discover.

### 5. Accessibility

- A vertical tab list: the column is `role="tablist"` (`aria-label` names what it switches, e.g. *Chart tools*), each
  tile a tab; ↑ / ↓ move and select, Home / End jump to the ends; only the selected tile is in the tab order.
- Give the panel `role="tabpanel"` and its id to `aria-controls`; label the panel with the group's name.

## Overview

### Tabs on their side

Use a side nav when a panel has **3–7 groups** and its content needs the width — a tools sheet or a settings area. The column sits on the left, the selected group's content on the right, and each scrolls on its own. For 2–5 sections across the top of a screen or sheet use **Tabs**; to go to another screen use list rows or the bottom navbar.

### Tiles

Tiles are square, fill the 96 column (80 with 8 on either side) and sit 10 apart, starting 16 from the top. A hairline separates the column from the panel. Unselected tiles are outlined; the selected tile is filled with no outline — grey by default, or the blue discover tint where the product's tools are blue. One tone per product.

Labels are one word, two at most. A **NEW** flag can mark one new group for a release or two — Zing yellow only for something new in Zing.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `items` | `SideNavItem[]` |  | { value, label, icon, flag? } — one per tile, top to bottom. icon: one 24 icon; flag: a short corner flag ("NEW"). |
| `value` | `string` |  | The selected item's value. Exactly one tile is selected. |
| `onChange` | `(value) => void` |  | Called on tap and on ↑ / ↓ / Home / End. |
| `aria-label` | `string` |  | Required: what the nav switches, e.g. "Chart tools". |
| `selectedTone` | `'neutral' \| 'discover'` | `'neutral'` | The selected tile's fill: surface/secondary, or the discover tint (navy in dark). |
| `flagTone` | `'discover' \| 'zing'` | `'discover'` | The corner flags' colour. |
| `aria-controls` | `string` |  | Id of the panel (role="tabpanel") the nav switches. |

## Tokens used

- `border/light (outline, divider)`
- `surface/secondary · accent/discover-light (selected)`
- `content/primary · secondary`
- `surface/accent/discover-default · zing-default (flag)`
- `state-layer/dark-hover`
- `radius/12 · 08`
- `spacing/10 · 08 · 04 · 02 · 06`
- `icon-size/24`
- `Label/14 · Heading/10`

## Recent changes

- **1.0.0** (2026-10-27) New: Side nav: Side nav: a column of square tiles (icon over label) that switches the panel beside it — for tools sheets and settings areas with 3–7 groups. Unselected tiles are outlined, the selected one is filled (grey, or the discover tint). A tile can carry a corner flag such as NEW, in discover blue or Zing yellow. Works as a vertical tab list: ↑ / ↓ move and select, Home / End jump to the ends, only the selected tile is in the tab order. From the Lemonn F&O Scalper chart-tools sheet. Not in Figma yet.
