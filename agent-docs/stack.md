# Stack

> Items in a column or a row with a gap from the spacing scale — the same as a Figma auto-layout frame.

- Group: Layout
- Lifecycle: done
- Version: 1.0.0
- Source: `src/components/Stack`
- Also called: Auto layout, flex, row, column, VStack, HStack, spacer, gap

## Import

```tsx
import { Stack } from './components/Stack' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Stack } from './components/Stack'` · Figma: an auto-layout frame (no component) · Docs: `#/stack`

Items in a column or a row with a gap from the spacing scale. No padding, no fill.

---

### 1. When to use it

- Space between items, nothing else → **Stack**.
- Padding, a fill, rounded corners or a border as well → `Section`. Padding only → `Inset`. Equal columns → `Grid`.
- A thing with a job of its own (something you open, a summary, grouped details) → `Card`.
- Never spacer elements or margins on items: the gap belongs to the container.

### 2. Props

| Prop | Values | Default | Figma |
|---|---|---|---|
| `direction` | `vertical` · `horizontal` | `vertical` | Direction ↓ / → |
| `gap` | `00` `04` `08` `12` `16` `24` `32` `40` `48` `64` | `00` | Gap `spacing/<n>` |
| `align` | `stretch` · `start` · `center` · `end` | `stretch` | Alignment across the axis (stretch = children Fill) |
| `justify` | `start` · `center` · `end` · `between` | `start` | Packed / Space between |
| `wrap` | boolean | `false` | Wrap (same gap between lines) |
| `as` | block element (`div`, `ul`, `section`…) | `div` | — |

### 3. Which gap

| Gap | Between |
|---|---|
| 4 | a title and its supporting text |
| 8 | sibling controls; a label and its field |
| 12 | parts of a row (icon · text · action); cards in a grid |
| 16 | a section heading and its card; cards in a list |
| 24 | sections (32 for a bigger break) |

Only the 4px scale above — never 02, 06, 10… (DESIGN_SYSTEM.md §1).

## Overview

### Figma auto layout, in code

A Stack is an auto-layout frame with no fill and no padding: **direction** (vertical or horizontal), the **gap** between items (a spacing variable, 0 to 64), **alignment** across the axis (stretch, start, centre, end), **justification** along it (packed at the start, centre or end, or space between) and **wrap**. Gaps always come from the container — never spacer layers or margins on the items.

### Which gap

4 between a title and its supporting text · 8 between sibling controls and from a label to its field · 12 between the parts of a row (icon · text · action) and between cards in a grid · 16 from a section heading to its card · 24 between sections (32 for a bigger break).

### Stack, Section, Inset or Grid

Only spacing between items → **Stack**. Padding, a fill or rounded corners as well → **Section**. Only padding (the 16 page gutter) → **Inset**. Equal-width columns → **Grid**. Anything with a border and a job of its own (a summary, an order you can open) is a **Card**.

Every value is a token suffix (`gap="16"` → `var(--l3-spacing-16)`), the same lists as the Build page's `schema.ts`. `as` renders another block element (`ul`, `section`…); `ref` and HTML attributes pass through.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `direction` | `'vertical' \| 'horizontal'` | `'vertical'` | Figma auto-layout direction. |
| `gap` | `'00' \| '04' \| '08' \| '12' \| '16' \| '24' \| '32' \| '40' \| '48' \| '64'` | `'00'` | Space between items (spacing/<step>). |
| `align` | `'stretch' \| 'start' \| 'center' \| 'end'` | `'stretch'` | Across the axis. Stretch fills the width (vertical) or height (horizontal). |
| `justify` | `'start' \| 'center' \| 'end' \| 'between'` | `'start'` | Along the axis; between = Space between. |
| `wrap` | `boolean` | `false` | Items flow onto the next line; the same gap is used between lines. |
| `as` | `'div' \| 'section' \| 'ul' \| 'ol' \| 'li' \| …` | `'div'` | The element to render. |

## Tokens used

- `spacing/00 → 64 (gap)`

## Recent changes

- **1.0.0** (2026-10-21) New: Stack: Stack: items in a column or a row with a gap from the spacing scale, cross-axis alignment, justification (incl. space between) and wrap. No Figma component: in Figma this is an auto-layout frame with its spacing, radius and fill bound to variables. Every option matches a Figma auto-layout setting. Values are token suffixes, the same lists as the Build page schema (steps, radii, aligns, backgrounds), so a Build design maps 1:1 and L3 Studio reads the options from the prop types. Shared types in components/layout.ts.
