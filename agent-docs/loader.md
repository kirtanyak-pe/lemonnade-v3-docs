# Loader

> A spinning ring for a step that is running and has nothing to draw yet — verifying a code, closing positions.

- Group: Feedback & status
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5587-2131
- Source: `src/components/Loader`
- Also called: Spinner, loading indicator, activity indicator, progress ring, busy

## Import

```tsx
import { Loader } from './components/Loader' // path relative to src/
```

## Usage rules (USAGE.md)

`import { Loader } from './components/Loader'` · Figma: "L3: Loader" (5587:2131) · Docs: `#/loader`

An indeterminate spinning ring: a `border-light` track with a `content-primary` arc (≈ 108°, round ends — the same
look as Button's built-in loader). It says "something is running", never how far along it is.

---

### 1. When to use it

- A **step that's running** with nothing to draw yet: verifying an OTP, closing positions, pausing trading, placing a
  basket of orders.

Don't use it for:
- **Loading content** (a list, a card, a chart, a screen) → `Skeleton` / `SkeletonListRow` / `SkeletonCard` in the
  same layout. Never a loader on a blank screen when a skeleton could show the layout.
- **An action that's working** (Save, Place order) → `Button loading` (keeps its width, shows the loader).
- **Known progress** (step 2 of 4, 60% uploaded) → `ProgressBar`.

### 2. Sizes

| Size | Figma | Where |
|---|---|---|
| `sm` (default) | Size = Small · 16 | Inline, `spacing-08` before a short Label/14 line that says what's running: *Verifying…* |
| `lg` | Size = Large · 96 | The centre of a loading sheet, `spacing-24` above a Heading/16 line: *Closing open positions* |

- There is no middle size yet. If a real screen needs one, ask — don't scale the loader by hand.
- Always **say in words what's running**, next to the loader. The ring alone isn't enough.

### 3. Loading sheet (Large)

- A `BottomSheet` with only the loader and one Heading/16 line, centred; padding 16 top / 32 bottom.
- **No buttons, no header**, and it **can't be dismissed** (backdrop tap, drag and Esc do nothing). It moves on by
  itself to the next step or to the result (a `BottomSheetHeader size="lg"` sheet, or a toast).
- Several running steps in a row (*Closing open positions* → *Pausing F&O trades for today*) reuse the same sheet and
  change only the text.

### 4. Colour and themes

- Track `border-light`, arc `content-primary` — tokens only, so it follows every product and mode (Lemonn, CS PRO,
  Kuber; light / dark; ♿ Accessible). Don't recolour it (no brand-coloured loaders); on a filled button use the
  Button's own loader.

### 5. Accessibility

- `role="status"` with a visually hidden `label` (default *Loading*) — say what's running: `label="Verifying OTP"`.
- When the same words are written next to it, pass `label={null}`: the loader is hidden from screen readers, and the
  element holding the text gets `role="status"` so it's announced once:
  `<span role="status"><Loader label={null} />Verifying…</span>`.
- It keeps turning under `prefers-reduced-motion`, 3× slower (it's a status, so some movement stays).

## Overview

### Loader, skeleton or button?

Use a loader only for a **step that's running** with nothing to draw yet: verifying an OTP, closing positions, pausing trading. Loading content (a list, a card, a chart) is a **Skeleton** in the same layout, and an action that's working is the **Button's Loading state**. Never show a loader on a blank screen when a skeleton could show the layout.

### Small or Large

**Small** (16) sits inline, 8 before a short line of text that says what's happening (“Verifying…”). **Large** (96) is the centre of a loading sheet, 24 above a Heading-16 line (“Closing open positions”); that sheet has no buttons, can't be dismissed and moves on by itself. Always say in words what's running — the ring alone isn't enough. It turns slowly for people who turn off animations.

`role="status"` with a hidden `label` (default “Loading”). When the same words are written next to it, pass `label={null}` and put `role="status"` on the element holding the text, so it's announced once.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `size` | `'sm' \| 'lg'` | `'sm'` | Figma Size: Small 16 (inline) · Large 96 (loading sheet). |
| `label` | `string \| null` | `'Loading'` | Screen-reader text (role="status"). null = hidden, when visible text next to it says the same. |

## Tokens used

- `border/light (track)`
- `content/primary (arc)`
- `size/16 · 96`
- `spacing/02 · 06 (stroke)`
- `motion-duration-spin (local)`

## Recent changes

- **1.0.0** (2026-10-15) New: Loader: Loader: a spinning ring for a step that is running with nothing to draw yet. Small (16) inline next to text such as “Verifying…”, Large (96) in a loading sheet such as “Closing open positions”. Track border-light, arc content-primary, in every theme. New local token motion-duration-spin (800ms, one turn — the same speed as the Button loader). Figma has no motion variables. role="status" with a hidden label (default “Loading”); label={null} when visible text next to it already says it. Turns 3× slower under reduced motion. New Figma component L3: Loader (Size = Small · Large) on its own Loader page under Feedback & status, with size, stroke and colours bound to variables, plus inline and loading-sheet examples.
