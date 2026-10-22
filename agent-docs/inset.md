# Inset

> Padding around its content and nothing else — usually the 16 page gutter.

- Group: Layout
- Lifecycle: done
- Version: 1.0.0
- Source: `src/components/Inset`
- Also called: Padding, gutter, margin, container, page padding

## Import

```tsx
import { Inset } from './components/Inset' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Inset } from './components/Inset'` · Figma: a frame with padding (no component) · Docs: `#/inset`

Padding around its content and nothing else — no fill, no gap, no layout of its own.

---

### 1. When to use it

- The **16 page gutter** around content that isn't edge to edge: `<Inset paddingInline="16" paddingBlock="00">`.
- Padding around a single block (default 16 all round).
- Need a gap, a fill or a radius too → `Section`.

### 2. Props

| Prop | Values | Default | Figma |
|---|---|---|---|
| `padding` | spacing step `00`…`64` | `16` | Padding, all sides |
| `paddingInline` | spacing step | — (= `padding`) | Horizontal padding |
| `paddingBlock` | spacing step | — (= `padding`) | Vertical padding |
| `as` | block element | `div` | — |

### 3. Don't

- Don't wrap edge-to-edge parts (Actionbar, Tabs, ButtonGroup, plain ListCell, flat Card) — they bring their own 16.
- Pill tabs: never gutter 16 + the tabs' own 16 — the first chip lines up with the gutter.

## Overview

### Padding only

An Inset adds **padding** — 16 all round by default, or left/right and top/bottom separately — and changes nothing else: no fill, no gap, no layout of its own. Its most common job is the **16 page gutter** (left and right 16, top and bottom 0) around content that doesn't run edge to edge, so rounded cards line up with section titles.

### When not to use it

Edge-to-edge parts — Actionbar, Tabs, Button dock, plain List cells, flat Cards — bring their own 16 inset; don't wrap them in another. Pill tabs line up their first chip with the gutter: never gutter 16 + the tabs' own 16. Need a gap or a fill as well? Use a **Section**.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `padding` | `spacing step` | `'16'` | Padding on every side (spacing/<step>). |
| `paddingInline` | `spacing step` |  | Left and right; overrides padding. |
| `paddingBlock` | `spacing step` |  | Top and bottom; overrides padding. |
| `as` | `'div' \| 'section' \| …` | `'div'` | The element to render. |

## Tokens used

- `spacing/00 → 64 (padding)`

## Recent changes

- **1.0.0** (2026-10-21) New: Inset: Inset: padding only, 16 all round by default; left/right and top/bottom can differ (the 16 page gutter). No Figma component: in Figma this is an auto-layout frame with its spacing, radius and fill bound to variables. Every option matches a Figma auto-layout setting. Values are token suffixes, the same lists as the Build page schema (steps, radii, aligns, backgrounds), so a Build design maps 1:1 and L3 Studio reads the options from the prop types. Shared types in components/layout.ts.
