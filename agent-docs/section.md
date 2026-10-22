# Section

> A vertical block with padding, a gap and, if it needs one, a fill, rounded corners and a border.

- Group: Layout
- Lifecycle: done
- Version: 1.0.0
- Source: `src/components/Section`
- Also called: Container, block, box, panel, frame, group, wrapper

## Import

```tsx
import { Section } from './components/Section' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Section } from './components/Section'` · Figma: an auto-layout frame (no component) · Docs: `#/section`

A vertical block with padding, a gap and, when it needs one, a fill, rounded corners and a border. The same container
as a section on the Build page (figma-react-app), with the same options.

---

### 1. When to use it

- A part of the screen that needs padding and spacing between its items → **Section**.
- Only a gap → `Stack`. Only padding → `Inset`.
- A summary, grouped details, something you can open → `Card` (it has the card rules: 12 padding, border, press).
- A section title is always a `SectionHeader` inside the section, 16 above its content.

### 2. Props

| Prop | Values | Default | Figma |
|---|---|---|---|
| `padding` | spacing step `00`…`64` | `00` | Padding, all sides |
| `paddingBlock` / `paddingInline` | spacing step | — (= `padding`) | Vertical / horizontal padding |
| `gap` | spacing step | `00` | Gap |
| `align` | `stretch` · `start` · `center` · `end` | `stretch` | Alignment |
| `surface` | `none` · `primary` · `secondary` · `tertiary` | `none` | Fill `surface/<name>` |
| `radius` | `00` `08` `12` `16` `24` | `00` | Corner radius `radius/<n>` |
| `border` | boolean | `false` | Stroke `border/light` 1 |
| `as` | block element (`div`, `section`…) | `div` | — |

### 3. Fills

- Default **no fill** — the screen's surface shows through.
- A large block that stands apart: `surface="primary"` + `border`.
- Grey (`secondary` / `tertiary`) only for **small** highlight boxes inside a card. A large grey area only when it's
  intentional and high-emphasis (DESIGN_SYSTEM.md 7.2).
- Rounded sections sit inside the 16 page gutter, like cards.

## Overview

### A padded block

A Section is a vertical auto-layout frame with **padding** (all round, or top/bottom and left/right separately), a **gap**, **alignment**, a **surface** fill (none, primary, secondary, tertiary), a **corner radius** and an optional **border** (border/light). It is the same container as a section on the Build page, with the same options.

### Fills

Most sections have **no fill**: the screen's surface shows through. A large block that needs to stand apart is surface/primary with a border. Grey (secondary, tertiary) is for **small** highlight boxes inside a card; a big grey area only when it's meant to stand out. A section title is always a **Section header** inside the section, 16 above its content.

### Section or Card

Use a **Card** for a thing with its own job — something you can open, a summary, grouped details. Use a Section to lay out a part of the screen: the page gutter and the space between items, sometimes with a background.

`paddingBlock` / `paddingInline` override `padding` for one axis. Default element is `div`; pass `as="section"` for a titled region.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `padding` | `spacing step` | `'00'` | Padding on every side (spacing/<step>). |
| `paddingBlock` | `spacing step` |  | Top and bottom; overrides padding. |
| `paddingInline` | `spacing step` |  | Left and right; overrides padding. |
| `gap` | `spacing step` | `'00'` | Space between the items inside. |
| `align` | `'stretch' \| 'start' \| 'center' \| 'end'` | `'stretch'` | How items line up across the width. |
| `surface` | `'none' \| 'primary' \| 'secondary' \| 'tertiary'` | `'none'` | Fill (surface/<name>). |
| `radius` | `'00' \| '08' \| '12' \| '16' \| '24'` | `'00'` | Corner radius (radius/<step>). |
| `border` | `boolean` | `false` | A border/light hairline. |
| `as` | `'div' \| 'section' \| …` | `'div'` | The element to render. |

## Tokens used

- `spacing/00 → 64 (padding, gap)`
- `radius/00 · 08 · 12 · 16 · 24`
- `surface/primary · secondary · tertiary`
- `border/light`

## Recent changes

- **1.0.0** (2026-10-21) New: Section: Section: a vertical block with padding (all round or per axis), a gap, alignment, a surface fill (none, primary, secondary, tertiary), corner radius and an optional border/light border. No Figma component: in Figma this is an auto-layout frame with its spacing, radius and fill bound to variables. Every option matches a Figma auto-layout setting. Values are token suffixes, the same lists as the Build page schema (steps, radii, aligns, backgrounds), so a Build design maps 1:1 and L3 Studio reads the options from the prop types. Shared types in components/layout.ts.
