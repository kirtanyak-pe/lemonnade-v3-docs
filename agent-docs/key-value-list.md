# Key-value list

> A key-value list shows facts as label → value: order details, fees and charges, account info, receipts — with an optional total.

- Group: Data display
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5528-264
- Source: `src/components/KeyValueList`
- Also called: Description list, details, summary, label value, fact list, receipt, breakdown

## Import

```tsx
import { KeyValueList, KeyValueRow } from './components/KeyValueList' // path relative to src/
```

## Overview

### When to use it

For facts people read, not tap: what an order was, what it cost, account and bank details, a receipt. If a row opens something, it's a List cell, not a key-value row.

### Horizontal or stacked

Horizontal (label left, value right) for short values like prices and quantities. Stacked (label above value) when the value is long — an address, a UPI ID, a bank name — so it wraps instead of squeezing the label.

### Extras on a row

- **Info (ⓘ)** after the label explains a term in a tooltip (Charges → what's included) or opens a breakdown sheet.
- **Copy** after the value for IDs people paste elsewhere: order ID, UPI ID, account number. It confirms with a tick.
- **Caption** under the value for one extra fact (LTP under the average price).
- **Green / red** only for gains and losses (P&L, returns) — never for plain amounts.

### Plain or card, and the total

Plain sits in the 16 page margin. Card boxes it like a static card (border, no shadow — it isn't tappable). Turn on Total for a sum at the end: a divider, then a bolder row. Medium rows are 12 apart, Small rows 8.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `variant` | `'plain' \| 'card'` | `'plain'` | List · Figma isCard. |
| `size` | `'md' \| 'sm'` | `'md'` | List · Figma Size; rows take it unless they set their own. |
| `total` | `{ label, value, caption?, tone? }` |  | List · Figma 👁️ Total: divider + isTotal row at the end. |
| `aria-label` | `string` |  | List · what the facts are about (“Order summary”). |
| `label / value` | `ReactNode` |  | Row · Figma ✏️ Label / ✏️ Value. |
| `layout` | `'horizontal' \| 'stacked'` | `'horizontal'` | Row · Figma Layout. |
| `caption` | `ReactNode` |  | Row · Figma 👁️ Caption under the value. |
| `info / onInfo` | `ReactNode / () => void` |  | Row · Figma 👁️ Info: tooltip text, or open something bigger. infoLabel names the button. |
| `copy` | `boolean \| string` |  | Row · Figma 👁️ Icon-R (copy): copies the value (or this string). |
| `valueEnd` | `ReactNode` |  | Row · Figma 👁️ Icon-R with another icon, e.g. an edit icon button. |
| `tone` | `'up' \| 'down'` |  | Row · profit green / loss red value, for gains and losses only. |
| `total (row)` | `boolean` | `false` | Row · Figma isTotal (the list total prop sets it for you). |

## Tokens used

- `content/primary · secondary · tertiary`
- `content/accent/indicator/up · down`
- `surface/default`
- `border/light`
- `radius/12`
- `spacing/02 · 04 · 08 · 12 · 16`
- `Description/10 · 12 · 14`
- `Label/12 · 14 · 16`
- `size/tap-target`

## Recent changes

- **1.0.0** (2026-10-11) New: Key-value list: Facts as label → value for order details, fees, account info and receipts. Plain or card, Medium or Small, with an optional total under a divider. Rows: horizontal or stacked (long values wrap), ⓘ after the label, copy after the value, a caption under it, green / red for gains and losses. <KeyValueList> (a <dl>) + <KeyValueRow>; info opens a Tooltip, onInfo opens anything; copy confirms with a tick and a screen-reader “Copied”. L3: Key-value row (Size × Layout × isTotal) and L3: Key-value list (isCard × Size, Rows slot, 👁️ Total) on the new Key-value list page.
