# Price change

> A signed change — price, P&L or returns — in green when up and red when down.

- Group: Data display
- Lifecycle: wip
- Status: Figma synced
- Version: 1.1.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5391-79
- Source: `src/components/PriceChange`
- Also called: Change, delta, percent change, returns, gain/loss, P&L change, LTP change

## Import

```tsx
import { PriceChange } from './components/PriceChange' // path relative to src/
```

## Usage rules (USAGE.md)

`import { PriceChange } from './components/PriceChange'` · Figma: "L3: Price change" (5391:79) · Docs: `#/price-change`

A signed change — a price move, P&L or returns — in green when up and red when down. Pass the signed number; the sign,
the colour and what screen readers hear all come from it, so they can never disagree.

---

### 1. Show the sign, not an arrow. Never both.

- Every price move, as a % or as an amount, shows its sign in the number — **+1.24%**, **−₹320.00** — in the indicator
  green or red. No change shows no sign, in grey.
- **No ▲ / ▼ arrow.** The sign plus the colour already say the direction; an arrow on top is redundant. This holds
  everywhere: lists, cards, headline numbers (LTP, Total P&L).
- The `arrow` prop is **deprecated** — never pass it. (Figma: keep 👁️ Arrow off.)
- Never write a change by hand (`+1.2%` in a span, colour set yourself) — always `PriceChange`.

### 2. Size

| Where | Size |
|---|---|
| Rows, cards, under a price in a list | `sm` (Label/12) |
| Next to a Label/14–16 value | `md` (Label/14) |
| Next to a big number (LTP, Total P&L) | `lg` (Label/16) |

### 3. Amount + percentage

For an absolute change with its percentage, write both: `<PriceChange value={252.89} unit="number" percent={0.05} />`
→ +252.89 (0.05%). Currency: `unit="currency"` (₹ with Indian grouping).

```tsx
<PriceChange value={s.changePct} size="sm" />                           // +1.24%
<PriceChange value={-320} unit="currency" size="md" />                  // −₹320.00
<PriceChange value={-0.92} unit="number" percent={-0.22} size="lg" />   // −0.92 (−0.22%)
```

## Overview

### The sign decides the colour

Up is green with “+”, down is red with “−”, and no change is grey with no sign. In Figma you pick the Direction and type only the number in ✏️ Value, so the sign and the colour can never disagree — a falling price can't end up green.

Pass the signed number as `value`; direction, sign and colour all come from it. Screen readers hear “up” / “down” / “unchanged” first.

### Sizes

**Small** (Label-12) in rows and cards, **Medium** (Label-14), **Large** (Label-16) next to a big number like Total P&L.

**Show the sign, not an arrow — never both.** +1.24% / −₹320 in green or red already says the direction, so there's no ▲ / ▼, even on headline numbers (keep Figma 👁️ Arrow off). For an absolute change with a percentage, write both: +252.89 (0.05%).

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `value` | `number` |  | The signed change; sets direction, sign and colour. |
| `unit` | `'percent' \| 'currency' \| 'number'` | `'percent'` | How the absolute value is written. |
| `percent` | `number` |  | Optional % in brackets after an absolute change. |
| `decimals` | `number` | `2` | Decimal places. |
| `size` | `'sm' \| 'md' \| 'lg'` | `'sm'` | Figma Size. |
| `arrow` | `boolean` | `false` | Deprecated — show the sign, never an arrow. Figma 👁️ Arrow stays off. |

## Tokens used

- `content/accent/indicator/up-default · down-default`
- `content/secondary (flat)`
- `Label/12 · 14 · 16`

## Recent changes

- **1.1.0** (2026-11-03) The sign, never an arrow: A price move shows its sign in the number (+1.24% / −₹320) in green or red — no ▲ / ▼ arrow, even on headline numbers. The sign and colour already say the direction; never both. Usage rules written. L3: Price change: keep 👁️ Arrow off on every instance. arrow is deprecated (still compiles, never pass it); removed from every docs example, the playground and the screen generator. audit:ui flags <PriceChange arrow>.
- **1.0.0** (2026-10-09) New component: Price change: a signed change in green (up, +), red (down, −) or grey (no change), in three sizes with an optional ▲/▼ arrow. The sign and colour come from the direction, so they can never disagree. New Figma component L3: Price change (Direction × Size, ✏️ Value without the sign, 👁️ Arrow). Screen readers hear “up / down / unchanged” and the value; colour is never the only signal.
