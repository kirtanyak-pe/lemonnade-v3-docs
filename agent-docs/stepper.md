# Stepper

> A number with − and + buttons, for quantity and lots in the order pad.

- Group: Input & control
- Lifecycle: done
- Status: Figma synced
- Version: 1.3.1
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5374-18184
- Source: `src/components/Stepper`
- Also called: Quantity input, counter, number input, lot picker, spinner

## Import

```tsx
import { Stepper } from './components/Stepper' // path relative to src/
```

## Overview

### Small or Large

**Small** sits inline in an order-pad row, next to its label (“Quantity (100 = 1 Lot)”): a 104×28 grey pill with 20px square buttons. **Large** stands on its own, like in Scalp Pro: round outlined buttons, a bigger value and an optional sublabel such as “91 Lots”.

### Limits

Each tap adds or removes one step — usually the lot size. At the lowest value the − button turns grey (State = Disabled), and the same for + at the highest. Never let the value go below one lot.

`min`, `max` and `step` clamp the value and disable the buttons. The Small buttons are 20px but their tap area is 48px.

### Type a value

Tapping the number opens the number keyboard, so a big quantity doesn't take dozens of taps. The value looks the same until it's tapped; then a focus ring shows it's being typed into. It keeps its format while typing — ₹45,000 stays ₹45,000, never 45000. Done, Enter or tapping away applies it — kept between the lowest and highest value and rounded to the nearest step, so lots stay whole (250 with lots of 100 becomes 300). Esc or an empty field puts the old value back.

When only some amounts are allowed and people should know why — capital in multiples of ₹10K, say — don't round it for them: show the **error state** (the value turns red, the Small pill gets a red outline), say what's allowed right next to it, and keep the main button disabled until it's fixed. − and + always land on an allowed step. For big amounts (₹4,00,000) use the **full-width** Large stepper: − and + at the ends, the amount centred.

The number is an `<input role="spinbutton">` with `inputMode` numeric (decimal when `step` has decimals) and `enterKeyHint="done"`; ↑ / ↓ step the value. Turn typing off with `editable={false}` (the value becomes an `<output aria-live="polite">`). On iOS Safari a field under 16px zooms the page on focus — the app's viewport should set `maximum-scale=1` or the screen should handle it.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value` | `number` |  | Current value. |
| `onChange` | `(value: number) => void` |  | Called with the new, clamped value. |
| `min · max` | `number` | `0 · Infinity` | Limits; the − / + button disables at each. |
| `step` | `number` | `1` | Amount per tap (e.g. the lot size); typed values round to it. |
| `size` | `'sm' \| 'lg'` | `'sm'` | Figma Size: Small · Large. |
| `sublabel` | `ReactNode` |  | Large only: a line under the value, e.g. "91 Lots". |
| `format` | `(value) => string` |  | How the value is shown; default Indian grouping. |
| `editable` | `boolean` | `true` | Tap the number to type a value (number keyboard). Applied on Enter / blur: clamped to min–max and rounded to the nearest step; Esc reverts. |
| `autoCorrect` | `boolean` | `true` | False: a typed value is passed on as is (not rounded or clamped) — validate it and set invalid. |
| `invalid` | `boolean` | `false` | Error state: red value (and outline on Small). |
| `aria-describedby` | `string` |  | Id of the hint or error message next to it. |
| `fullWidth` | `boolean` | `false` | Stretch to the container: − / + at the ends, the value centred. |
| `label` | `string` |  | Accessible name of the group, e.g. "Quantity". Required. |
| `disabled` | `boolean` |  | Disables both buttons. |

## Tokens used

- `surface/secondary (Small)`
- `surface/inverted + surface/default icon (Small buttons)`
- `surface/primary + border/intense (Large buttons)`
- `surface/disabled + content/disabled (at the limit)`
- `content/accent/error-default + border/accent/error-default (error)`
- `Heading/12 · 14`
- `Label/10 (sublabel)`
- `size/20 · 32 · 48 · tap-target`
- `radius/04 · 08 · full`
- `spacing/02 · 04`

## Recent changes

- **1.3.1** (2026-10-13) Typing keeps the format: While typing, the value keeps its format (₹45,000, not 45000), so it looks the same at rest, while typing and in the error state. Clearing the field shows the old value faded until you type.
- **1.3.0** (2026-10-13) Error state · full width: Error state: when a typed amount isn’t allowed, the value turns red (and the Small pill gets a red outline). Say why right next to it. Full width: the stepper can stretch to its container — − and + at the ends, the value centred — so big amounts don’t crowd the buttons. − and + from an amount that isn’t on a step land on the nearest step (45,000 → 40,000 or 50,000 with steps of 10,000). `autoCorrect` (default true; false passes a typed value on as is, for the screen to validate), `invalid`, `aria-describedby`, `fullWidth`.
- **1.2.0** (2026-10-13) Type a value: Tap the number to type a value with the number keyboard. Done or tapping away applies it, kept between the limits and rounded to the nearest step (lots stay whole); Esc or an empty field puts the old value back. editable (default true) — the value is an <input role="spinbutton"> with inputMode numeric / decimal and enterKeyHint done; ↑ / ↓ step it. L3: Stepper has State = Default · Focused (Small and Large): Focused shows the number in a 2px border-dark ring with a caret, same size as Default. Use it in mockups of someone typing a quantity.
