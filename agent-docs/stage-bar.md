# Stage bar

> Shows which stage someone is on in a flow that spans several screens.

- Group: Feedback & status
- Lifecycle: wip
- Status: Figma synced
- Version: 1.0.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5625-2247
- Source: `src/components/StageBar`
- Also called: Stage indicator, step bar, segmented progress, step n of n, wizard progress, onboarding progress

## Import

```tsx
import { StageBar } from './components/StageBar' // path relative to src/
```

## Usage rules (USAGE.md)

`import { StageBar } from './components/StageBar'` · Figma: "L3: Stage bar" (5625:2247), segments "L3: base Stage
segment" (5625:2080) · Docs: `#/stage-bar`

Where someone is in a flow of a few stages that **spans several screens**: "Step n of n · Stage" (Label/12) over one
segment per stage.

---

### 1. When to use it

- A flow of **2–5 stages**, each one or more screens: account setup (identity → bank → F&O), a multi-screen form.

Don't use it for:
- **Every step on one screen** (a checklist, a request's journey) → `Steps`.
- **How much of one thing** is done or used → `ProgressBar`.

### 2. States

| Segment | Figma | Look |
|---|---|---|
| before `current` | State = Done | full success (surface/accent/success-default) |
| `current` | State = Current | first half success on the track |
| after `current` | State = To do | track (surface/tertiary) |

`current` runs 1 to `stages` (clamped). There's no "all done" — when the last stage finishes, leave the flow.

### 3. Content

- **Step text:** "Step 2 of 3" (built in; `stepText` for other languages), content/primary.
- **Stage:** the current stage's name as a short action — *Verify your identity*, *Add your bank account* —
  content/secondary, one line, ellipsis. Leave `stage` out when the screen title already says it.

### 4. Layout

- Goes in the **Actionbar ↓ Content bottom** slot (stays put while the page scrolls) and fills it.
- Brings its own padding: 4 top, 12 bottom, 16 sides. Don't wrap it in a Section or add margins.
- Segments are 4 high, 4 apart, equal width. Not tappable.

### 5. Accessibility

- The "Step 2 of 3 · Add your bank account" line is plain text, read as is. The segments are decorative
  (`aria-hidden`), so the text is always there — never hide both the step text and the stage.

## Overview

### Where you are across screens

Use the Stage bar in a flow of **2–5 stages** that spans several screens, like account setup: identity → bank → F&O. It reads **Step n of n** and the current stage's name, over one segment per stage. Done stages are green, the current one is half green, the rest are the grey track.

For the list of every step on one screen use **Steps**; for how much of one thing is done use a **Progress bar**.

### Placement

Put it in the **Actionbar ↓ Content bottom** slot, so it stays put while the page scrolls. It fills the slot and brings its own padding (4 top, 12 bottom, 16 sides). Turn the stage name off when the screen title already says it. It isn't tappable.

The text is a plain line (“Step 2 of 3 · Add your bank account”) that screen readers read; the segments are decorative. `stepText` replaces “Step n of n” for other languages.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `stages` | `2 \| 3 \| 4 \| 5` |  | Figma Stages: how many stages the flow has. |
| `current` | `number` |  | Figma Current: the stage the person is on (1 to stages). |
| `stage` | `string` |  | Figma ✏️ Stage: the current stage’s name. Leave out = 👁️ Stage off. |
| `stepText` | `string` | ``Step ${current} of ${stages}`` | The “Step n of n” text, for other languages. |

## Tokens used

- `surface/accent/success-default (done, current half)`
- `surface/tertiary (track)`
- `content/primary (Step n of n)`
- `content/secondary (stage)`
- `Label/12`

## Recent changes

- **1.0.0** (2026-10-27) New: Stage bar: Stage bar: “Step n of n · Stage” over one segment per stage, for a flow of 2–5 stages across several screens (account setup). Done stages are success, the current one half, the rest the track. Goes in the Actionbar ↓ Content bottom slot and brings its own padding (4 top, 12 bottom, 16 sides). The stage name can be turned off. The “Step n of n · Stage” line is read as plain text; the segments are decorative (aria-hidden). stepText replaces “Step n of n” for other languages. Figma components L3: base Stage segment (Done · Current · To do) and L3: Stage bar (Stages 2–5 × Current, ✏️ Stage / 👁️ Stage) on the ↪ Stage bar page, with examples and a DS index card.
