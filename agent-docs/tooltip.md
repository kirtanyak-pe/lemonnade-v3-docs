# Tooltip & coach mark

> A tooltip is a short dark label that explains an icon or a number. A coach mark is a light card that points out a new feature once.

- Group: Feedback & status
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.1
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5485-234
- Source: `src/components/Tooltip`
- Also called: Hint, info bubble, popover, onboarding tour, feature highlight, walkthrough, spotlight

## Import

```tsx
import { Tooltip, CoachMark } from './components/Tooltip' // path relative to src/
```

## Overview

### Tooltip

One short line (about 40 characters) that explains an icon or a number. It opens when people tap or press on the target, and closes when they tap anywhere else. Never put anything people need only in a tooltip — it's a hint, not the content. Turn on the ✕ (👁️ Close) for a hint that should stay until it's dismissed, like a tip on first visit.

### Coach mark

Use it once, to point out something new: a heading, one sentence on why it helps, and Next (or Got it). In a tour, show the step (“1 of 3”) and Skip. One at a time, and never more than three steps. It sits above or below its target and moves to stay 16 from the screen edge, with the arrow on the target.

Both place themselves: `placement` is a preference that flips if there's no room, and the bubble slides to stay 16 inside the nearest clipping container (the screen or sheet). The coach mark is a non-modal `role="dialog"` and focuses its main button; Esc skips.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `Tooltip · content` | `ReactNode` |  | Figma ✏️ Text: one short line. |
| `Tooltip · children` | `ReactNode` |  | The target — gets aria-describedby while open. |
| `Tooltip · placement` | `'top' \| 'bottom'` | `'top'` | Figma Placement (flips to fit). |
| `Tooltip · open / defaultOpen / onOpenChange` | `boolean / boolean / (open) => void` |  | Controlled or uncontrolled. |
| `Tooltip · onClose` | `() => void` |  | Figma 👁️ Close: shows ✕ and keeps it open until tapped. |
| `CoachMark · open` | `boolean` |  | Show it. |
| `CoachMark · heading / body` | `string / ReactNode` |  | Figma ✏️ Heading / ✏️ Body. |
| `CoachMark · step` | `string` |  | Figma ✏️ Step, e.g. “1 of 3”. |
| `CoachMark · nextLabel / onNext` | `string / () => void` | `'Next'` | Main button (“Got it” on the last step). |
| `CoachMark · onSkip / skipLabel` | `() => void / string` | `'Skip'` | Figma 👁️ Skip. Esc does the same. |
| `CoachMark · placement / autoFocus` | `'top' \| 'bottom' / boolean` | `'bottom' / true` | Figma Placement; focus the main button on open. |

## Tokens used

- `surface/inverted · primary`
- `content/inverted · primary · secondary`
- `Description/12`
- `Heading/14`
- `Label/12`
- `radius/08 · 12`
- `shadow/elevation-medium`
- `size/tooltip-max · coach-mark (local)`
- `spacing/04 · 08 · 12`

## Recent changes

- **1.0.1** (2026-10-11) Figma sync: tooltip spacing: Tooltip padding is 8 all round with the text inset 4 more on each side (still 12 from the edges); the ✕ sits 6 after the text and 8 from the edge. Coach mark body and step text use content-secondary at 60%.
- **1.0.0** (2026-10-11) New: Tooltip & coach mark: Tooltip: a short dark label for an icon or a number. Opens on tap, hover or focus; closes on tap-away or Esc. With 👁️ Close it shows ✕ and stays until dismissed. Coach mark: a light card that points out a new feature — heading, one sentence, optional “1 of 3” step, Skip and Next / Got it. Both sit above or below their target, flip when there isn't room and slide to stay 16 from the screen edge, with the arrow on the target. New Figma component L3: Tooltip (Type × Placement × Arrow, 👁️ Close) on its own Tooltip page.
