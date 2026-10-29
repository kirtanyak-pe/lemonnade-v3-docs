# Steps

> A vertical progress list for a process of a few steps — what’s done, where you are, what’s next.

- Group: Feedback & status
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5619-2148
- Source: `src/components/Steps`
- Also called: Stepper, progress steps, timeline, checklist, step indicator, progress tracker, onboarding steps

## Import

```tsx
import { Steps } from './components/Steps' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Steps } from './components/Steps'` · Figma: "L3: Steps" (5619:2148), rows "L3: base Step" (5619:2147) ·
Docs: `#/steps`

A vertical progress list: where someone is in a process of a few steps, read as **progress, not options**. Each row is
a marker, the step's name (Label/14) and an optional one-line description (Description/12). The line between markers
is green as far as they've got.

---

### 1. When to use it

- A **process of 2–5 steps** the person goes through once: account setup (identity → bank → F&O), fetching details
  (PAN → personal → bank), an order's or a request's journey (placed → under review → done).

Don't use it for:
- **Choices** → radio rows, `ListCell`, `Tabs`. Steps are never tappable.
- **How far along** one thing is (60% uploaded, margin used, step 2 of 4 across screens) → `ProgressBar`.
- **A single running step** with nothing to draw → `Loader`.

### 2. States

| State | Figma | Marker | Label |
|---|---|---|---|
| `done` | State = Done | 20 success disc with a white check | content/primary |
| `current` | State = Current | 12 success dot in a light-success halo (20) — "you are here" | content/primary |
| `loading` | State = Loading | `Loader` Small (16) — the current step while it runs | content/primary |
| `todo` | State = To do | 16 outline ring, border/intense | content/secondary |

- Order is always top to bottom: **done… → one current (or loading) → to do…**. At most one current/loading step.
- All done is fine (the last row done); none done is fine (the first row current).
- The line after a **done** step is 2 wide, success; after any other step it's a 1px border/light hairline. The last
  row has no line (Figma isLast = True — automatic in code).

### 3. Content

- **Label:** the step as a short noun phrase or an outcome — *Verify your identity*, *Bank account added*. Past tense
  once it's done (*Identity verified*) when the screen is a summary.
- **Description:** optional, one line — what it needs or how long it takes (*PAN and personal details · about 2 min*,
  *Usually 24–48 hours*). Leave it out when the label says enough; don't mix long and short descriptions for no reason.

### 4. Layout

- Plain: it sits inside the 16 page margin, no container. Put it in a static `Card` (padding 12) only when the screen
  needs a container (e.g. next to other cards).
- Space below each row: 24 after a done / current / loading step, 16 after a to-do step (built in). Don't add gaps.
- It's static — when a step finishes, change its `state` (done) and the next one's (current / loading); don't animate
  the rows in.

### 5. Accessibility

- An ordered list (`<ol>`) named by `label` (*Account setup*). The current or loading step has `aria-current="step"`.
- Each step's state is read after its name (*Verify your identity — done*); the colours are not the only cue — every
  state has its own marker shape.

## Overview

### Progress, not options

Use Steps for a process of **2–5 steps** someone goes through once: account setup, fetching their details, a request under review. It's read top to bottom — **done** steps, then **one current** step (or **loading** while it runs), then **to do**. The line is green as far as they've got. Steps are never tappable: for choices use radio rows or list cells, and for how far along one thing is use a Progress bar.

### States

**Done**: a green disc with a check. **Current**: a green dot with a light halo — “you are here”. **Loading**: a small Loader while that step runs. **To do**: an outline ring and a grey label. Each state has its own shape, so colour isn't the only cue. Leave 24 below a done or current step and 16 below a to-do step — it's built in.

An `<ol>` named by `label`; the current or loading step has `aria-current="step"` and every step's state is read after its name (“Verify your identity — done”). The last step's line is dropped automatically (Figma isLast).

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `steps` | `{ label: string; description?: ReactNode; state: StepState }[]` |  | Rows top to bottom (Figma L3: base Step). state: done · current · loading · todo. The last row is isLast. |
| `label` | `string` |  | What the list is, for screen readers (“Account setup”). |

## Tokens used

- `surface/accent/success-default (done)`
- `border/accent/success-default (current, line after done)`
- `surface/accent/success-light (halo)`
- `border/intense (to do)`
- `border/light (line)`
- `static/white (check)`
- `Label/14 · Description/12`

## Recent changes

- **1.0.0** (2026-10-19) New: Steps: Steps: a vertical progress list for a process of a few steps (account setup, fetching details, a request under review). Done = green disc with a check · Current = green dot with a light halo · Loading = a small Loader while that step runs · To do = an outline ring with a grey label. The line between markers is green after a done step, a hairline otherwise. 24 below done / current / loading rows, 16 below to-do rows. An ordered list named by label; the current or loading step has aria-current="step", and each step’s state is read after its name. New Figma components L3: base Step (State = Done · Current · Loading · To do × isLast) and L3: Steps (rows in its Steps slot) on a Steps page under Feedback & status, variables bound, with examples and a DS index card.
