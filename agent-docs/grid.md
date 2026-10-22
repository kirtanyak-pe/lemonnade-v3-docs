# Grid

> Items in 1 to 4 equal-width columns with one gap between columns and rows.

- Group: Layout
- Lifecycle: done
- Version: 1.0.0
- Source: `src/components/Grid`
- Also called: Columns, tiles, grid layout, two-up, shortcuts

## Import

```tsx
import { Grid } from './components/Grid' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Grid } from './components/Grid'` · Figma: auto layout with Wrap, fixed-width children (no component) · Docs: `#/grid`

Items in 1–4 equal-width columns with one gap between columns and rows.

---

### 1. When to use it

- Stat tiles, key numbers, option cards side by side → `columns={2}`.
- Small items such as icon shortcuts → `columns={3}` or `{4}`.
- Things people read one after another → a vertical `Stack` or `ListCell`s, not a Grid.

### 2. Props

| Prop | Values | Default | Figma |
|---|---|---|---|
| `columns` | `1` · `2` · `3` · `4` | `2` | Children per row |
| `gap` | spacing step `00`…`64` | `12` | Gap (columns and rows) |
| `align` | `stretch` · `start` · `center` · `end` | `stretch` | Alignment in each cell (stretch = same height per row) |
| `as` | block element (`div`, `ul`…) | `div` | — |

### 3. Notes

- Screens are 360 wide: with 16 gutters and a 12 gap, two columns are 158 each. More than 2 columns only for short
  content — text wraps inside its column and never widens it.

## Overview

### Equal columns

A Grid puts items in **1–4 equal columns** (Figma: auto layout with Wrap and fixed-width children) with one **gap** between columns and rows — 12 by default. Long text wraps inside its column; it never makes a column wider.

### How many columns

Screens are 360 wide. **2** columns for stat tiles, option cards and key numbers; **3–4** only for small items like icon shortcuts. A list of things people read one after another is a vertical Stack or List cells, not a Grid. With **stretch** every item in a row is as tall as the tallest one.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `columns` | `1 \| 2 \| 3 \| 4` | `2` | Equal-width columns. |
| `gap` | `spacing step` | `'12'` | Between columns and rows (spacing/<step>). |
| `align` | `'stretch' \| 'start' \| 'center' \| 'end'` | `'stretch'` | How items line up in their cell, top to bottom. |
| `as` | `'div' \| 'ul' \| …` | `'div'` | The element to render. |

## Tokens used

- `spacing/00 → 64 (gap)`

## Recent changes

- **1.0.0** (2026-10-21) New: Grid: Grid: 1–4 equal columns with one gap between columns and rows (12 by default) and alignment inside each cell. No Figma component: in Figma this is an auto-layout frame with its spacing, radius and fill bound to variables. Every option matches a Figma auto-layout setting. Values are token suffixes, the same lists as the Build page schema (steps, radii, aligns, backgrounds), so a Build design maps 1:1 and L3 Studio reads the options from the prop types. Shared types in components/layout.ts.
