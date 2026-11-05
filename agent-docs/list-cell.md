# List cell

> A row in a list: an icon, a label with an optional description, and something on the right — a chevron, a switch, a tag or a value.

- Group: Data display
- Lifecycle: wip
- Status: Figma synced
- Version: 1.10.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=4543-65400
- Source: `src/components/ListCell`
- Also called: List item, row, cell, settings row, menu item

## Import

```tsx
import { ListCell } from './components/ListCell' // path relative to src/
```

## Usage rules (USAGE.md)

`import { ListCell } from './components/ListCell'` · Figma: "L3: list cell" (4543:65400) · Docs: `#/list-cell`

A row in a list: an icon, a label with an optional description, and something on the right — a chevron, a switch, a
tag or a value. **A list cell is a card with specific content, so it follows the Card rules** (`Card/USAGE.md`).

---

### 1. When to use it

- Lists of things: holdings, watchlist (symbol · Sparkline · price + `PriceChange`), search results, history grouped
  by date.
- Settings and menus: navigation rows with a chevron, toggles with a `Switch`.
- Choices: the options in a select sheet (single select → `Radio` rows; multi-select → `Checkbox` rows).
- Facts in a review sheet: *Quantity 10*, *Price ₹2,948.60*, *Charges ₹23.10* (see `BottomSheet/USAGE.md` §7).

Don't use it for:
- One block of grouped content → `Card`.
- 2–4 options that should all be visible at once → pill `Tabs`.
- A status message → `Aerobar`.

### 2. Plain or card

| Row | Figma | Code | Looks |
|---|---|---|---|
| Plain | isPlain = True | `variant="plain"` (default) | a flat card: **no fill** (takes the colour of what it sits on), runs edge to edge |
| Card, not tappable | isPlain = False, isTappable = False | `variant="card"` | a static card: **no fill** + `border-light`, no shadow, radius 12 |
| Card, tappable | isPlain = False, isTappable = True | `variant="card"` + `onClick` / `href` / `as="label"` | a clickable card: `surface-primary` + `border-light` + `elevation-soft`, scales to 0.98 while pressed |
| Card, tappable, in a long list | same, no shadow | `variant="card"` + `onClick` / `href` + `raised={false}` | `surface-primary` + `border-light`, **no shadow**, still scales to 0.98 |

- **Long lists** (watchlists, market lists, search results, ETF lists) → flat tappable cards, `raised={false}`, no
  shadow. A few standalone cards may stay raised (the default). Never mix the two in one list.

- Card rows always sit inside a margin (16 from the screen edge, like any card). Plain rows run edge to edge in their
  container and keep their own 16 side padding.
- Sizes: `md` (default — min 48, 24px icons) and `sm` (Figma isSmall — min 32, 16px icons).
- **Density** (flat rows only, both sizes): `compact` (default — 8 above and below; 58px default rows, 54px small) or
  `breathable` (16 above and below; 74px default rows, 70px small). **Asset lists** — logo, name + company, price +
  `PriceChange` — use breathable; settings, menus and options stay compact. **Card rows never change** — they have no
  density (TypeScript won't take one). In Figma: the "📐 L3 → Density" mode on the list frame.

### 3. Tappable rows

- A tappable row is one tap target: `onClick` (a button) or `href` (a link) makes the **whole row** tappable, with a
  pressed tint.
- **Show where a row leads:** a tappable row gets a chevron (`iconRight`) or a control on the right. Never a tappable
  row with nothing to hint at it.
- **A row with a `Switch`, `Checkbox` or `Radio`** is `as="label"`, with the control in `trailing`: tapping anywhere on
  the row toggles it. Don't make that row a button or a link — a button can't contain another control (development
  warning).
- **Which control:** a `Switch` is a setting that applies immediately (not inside a form that needs Save); `Checkbox`
  rows for picking several; `Radio` rows (in a select sheet) for picking one.

### 4. Selected rows

When rows are a list of choices, the chosen one is `selected` (Figma isSelected = True).

- **Plain** row: a `surface-secondary` background.
- **Card** row: `border-light` becomes `border-dark`; nothing else changes.
- **Only tappable rows** can be selected (development warning otherwise). In a `label` row, the `Radio` / `Checkbox`
  carries the state for screen readers.
- One surface cue plus the control's mark — never stack cues, never colour alone, never a font-weight change.

### 5. Content

- **Label:** one line, short; it truncates with "…".
- **Description:** one line ending in "…" by default. `multiline` (Figma isMultiline) lets it wrap onto as many lines
  as it needs — for a setting explained in a sentence. The label still stays on one line.
- **Right side:** `iconRight` for a chevron; `trailing` for anything else — `Switch`, `Checkbox`, `Radio`, `Tag`, a
  value, a price + `PriceChange`. `trailing` comes before `iconRight` when both are set.
- **Left side:** `iconLeft` — an icon, a logo or an avatar; the slot sizes it (24 / 16).
- **Dots:** `dotLeft` / `dotRight` put an unread or new dot on the icon. It's only visual, so `dotLabel` says what
  screen readers hear (default "New").
- In a settings screen, destructive actions go last, as `tertiary` buttons.

### 6. Lists

**Stock / ETF row (recipe — no separate component).** Every stock, ETF or fund list uses the same card row:

```tsx
<ListCell
  variant="card"
  raised={false}                       // long list: no shadow
  onClick={() => openStock(s.symbol)}
  iconLeft={<Avatar size={40} src={s.logo} />}
  labelSize="14"                       // card lists: Label/14 (keeps the 40 logo and md height)
  label={s.symbol}                     // one line
  description={s.name}                 // Description/12, one line with …
  trailing={formatPrice(s.price)}      // a formatted string (₹ / $) — strings get the value style, Label/14
  caption={<PriceChange value={s.changePct} size="sm" />}   // +1.24% — sign, no arrow
/>
```

Symbol and price both Label/14, price over change right-aligned, 2 apart (the `caption` stack). **Stock card lists
sit 12 apart (`Stack gap="12"`)** — tighter than the 16 between other cards, so a long list reads as one list — inside
the 16 margin. A short list of a few highlighted stocks may drop `raised={false}`.

- A list under a heading: `SectionHeader`, then the rows 16 below it.
- Sections are separated by space, not lines. A divider goes only between rows of the same list.
- Loading: `SkeletonListRow` in the same layout. Empty: `EmptyState` with a way forward.

### 7. Anatomy (Figma properties → props)

| Figma | Prop | Notes |
|---|---|---|
| isPlain = True · False | `variant` `'plain' \| 'card'` | |
| isSmall | `size` `'md' \| 'sm'` | |
| 📐 L3 → Density mode: Compact · Breathable | `density` `'compact' \| 'breathable'` | flat rows only, both sizes — set the mode on the list frame (or one row); asset lists are breathable; card rows never change |
| isTappable | `onClick` · `href` · `as="label"` | `as="button"` / `"a"` / `"label"` / `"div"` can be set directly |
| isSelected | `selected` | tappable rows only |
| isMultiline | `multiline` | the variant is being added to the Figma library |
| ✏️ Label · ✏️ Description (👁️ Description) | `label` · `description` | |
| Icon-l (👁️ Icon - L) | `iconLeft` | |
| icon-r (👁️ Icon - R) | `iconRight` (chevron) or `trailing` (switch, tag, value) | |
| 👁️ Dot-L · 👁️ Dot-R | `dotLeft` · `dotRight` + `dotLabel` | |

---

### Code

```tsx
// Navigation: the whole row is tappable, the chevron shows where it leads
<ListCell label="Holdings" description="12 stocks" iconRight={<Icon icon={msChevronRight} />} onClick={openHoldings} />

// Setting: tap anywhere on the row to toggle; the description wraps
<ListCell
  as="label"
  label="Price alerts"
  description="Get notified when a stock in your watchlist moves more than 5% in a day"
  multiline
  trailing={<Switch defaultChecked />}
/>

// Single select in a sheet: Radio rows, the chosen one selected
{sorts.map((s) => (
  <ListCell key={s.value} as="label" label={s.label} selected={sort === s.value}
    trailing={<Radio name="sort" checked={sort === s.value} onChange={() => setSort(s.value)} />} />
))}

// A list of choices as card rows (inside a 16 margin): the chosen account gets border-dark
<ListCell variant="card" label="HDFC Bank ••4821" description="Primary" iconLeft={bankLogo}
  onClick={() => setAccount('hdfc')} selected={account === 'hdfc'} iconRight={<Icon icon={msChevronRight} />} />

// Asset list: breathable rows (16 above and below) — logo, name + company, price + change
<ListCell density="breathable" label="Tata Motors" description="Tata Motors Ltd" iconLeft={tataLogo}
  trailing={<span className={styles.price}>₹418.20 <PriceChange value={0.22} unit="percent" size="sm" /></span>}
  onClick={openTataMotors} />

// Facts in a review sheet: static rows, the value on the right
<ListCell label="Quantity" trailing="10" />

// Unread dot on the icon, with a name for screen readers
<ListCell label="Inbox" iconLeft={<Icon icon={msMail} />} dotLeft dotLabel="New messages"
  iconRight={<Icon icon={msChevronRight} />} onClick={openInbox} />
```

---

### Open questions

<!-- PENDING: how a divider between rows of the same list is drawn — ListCell has no divider option -->
<!-- PENDING: when to use the small (sm) row instead of the default -->
<!-- PENDING: grouping and section headers in long lists (DESIGN_SYSTEM §7.3 list patterns) -->

## Overview

### Plain or card

A list cell is a card with more specific content — dropdown options, settings, lists — so it follows the **Card rules**. Plain rows are flat cards: no fill, they take the colour of whatever they sit on, and they run edge to edge. Card rows (Figma isPlain=False) are rounded with border-light and always sit inside a margin (16 from the screen edge):

- **Not tappable** (a static card): no fill — it takes the colour it sits on — and border-light, no shadow.
- **Tappable** (a clickable card, Figma isTappable): surface-primary, border-light and elevation-soft, and it scales to 0.98 while pressed.

Both come in default (48) and small (32).

### Compact or breathable

Flat rows come in two densities, in both sizes. **Compact** (8 above and below) is the default — settings, menus, options. **Breathable** (16 above and below: 74px default rows, 70px small rows) is for **asset lists**: a logo, the name and company, the price and its change. Card rows — rounded, with a border — always keep their spacing. In Figma, set the **📐 L3 → Density** mode to Breathable on the list frame — every flat row inside follows.

### Selected rows

When rows are a list of choices, the chosen one is selected. A plain row gets a surface-secondary background; a card row swaps border-light for the darker border-dark and nothing else changes. Only tappable rows can be selected.

`selected` on a tappable row (`onClick`, `href` or `as="label"`). Announced as pressed (button) or current (link); in a label row the Radio or Checkbox carries the state.

### Side and two-line values

For orders and trades, a short coloured word after the name says the side — Long or Buy in green, Short or Sell in red. On the right, a value or status can carry a caption underneath: the time, the type or “Realised P&L”. Amounts never wrap; the name gets shortened first. Use small rows with compact spacing and a 24px logo for long histories.

### Tappable rows

A whole row can be tappable, with a pressed tint — show a chevron or a control on the right so people know. A row with a Switch or Checkbox toggles it when tapped anywhere — try “Biometric login”.

Use `as="button"` or `href` to make the whole row tappable with a pressed tint. Use `as="label"` with a Switch or Checkbox in `trailing` so tapping anywhere on the row toggles it — try “Biometric login”.

## Do / Don't

### Show where a row leads

- ✅ **Do:** Tappable rows get a chevron (or trailing control).
- ❌ **Don't:** Make rows tappable with nothing to hint at it.

## Options (tree)

List cell — Rows of settings, accounts, items

- **Variant**
  - `plain` — Full width, edge to edge.
  - `card` — A one-line row as a card.
- **Size**
  - `md`
  - `sm`
- **Tap behaviour** — as=
  - `button / a` — Whole row taps; show a chevron.
  - `label` — With a Switch or Checkbox in trailing.
  - `dotRight` — Unread marker with screen-reader text.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `label / description` | `ReactNode` |  | Figma "Label goes here" / "Type description". |
| `multiline` | `boolean` | `false` | Figma isMultiline: the description wraps onto as many lines as it needs instead of ending in "…". The label stays on one line. |
| `size` | `'md' \| 'sm'` | `'md'` | Figma isSmall: 48 / 32 min height, 24 / 16 icons. |
| `density` | `'compact' \| 'breathable'` | `'compact'` | Flat rows only (both sizes): compact (8 above and below) or breathable (16 — 74px default rows, 70px small) for asset lists. Card rows have no density. |
| `variant` | `'plain' \| 'card'` | `'plain'` | Figma isPlain: flat row (no fill, edge to edge), or bordered rounded card (inside a margin). Card rows follow Card: static = no fill + border-light; tappable = surface-primary + elevation-soft. |
| `selected` | `boolean` | `false` | Figma isSelected: the chosen row in a list of choices — plain → surface-secondary, card → border-dark. Tappable rows only. |
| `iconLeft / iconRight` | `ReactNode` |  | Figma Icon-L / Icon-R slots, sized for you. |
| `trailing` | `ReactNode` |  | Anything else on the right: Switch, Checkbox, Tag, value text. |
| `caption` | `ReactNode` |  | Figma Trailing caption: a second line under trailing, right-aligned (time, type, "Realised P&L"). A text trailing becomes the value (Label/14 · 12 small) and never wraps. |
| `labelSuffix / labelSuffixTone` | `ReactNode / 'up' \| 'down' \| 'neutral'` | `— / 'up'` | Figma Label suffix: a short coloured word right after the label (Long, Short, Buy). The label truncates first; the suffix stays. |
| `dotLeft / dotRight` | `boolean` | `false` | Figma Dot-L / Dot-R: unread dot on the icon. |
| `dotLabel` | `string` | `'New'` | What screen readers hear for the dot (it is otherwise only visual). |
| `as / href / onClick` | `'div' \| 'button' \| 'a' \| 'label'` |  | Makes the row tappable (button / a) or a label for a trailing control. |

## Tokens used

- `surface/primary (tappable card)`
- `surface/secondary (plain selected)`
- `border/light`
- `border/dark (card selected)`
- `shadow/elevation-soft (tappable card)`
- `motion/scale/press-default`
- `content/primary · secondary`
- `content/accent/discover (dot)`
- `Label/14 · 16`
- `Description/12`
- `radius/12 · full`
- `icon-size/16 · 24`
- `state-layer/* (tappable rows)`

## Recent changes

- **1.10.0** (2026-11-04) Softer shadow on tappable card rows: Tappable card rows use elevation-soft instead of elevation-low, like clickable cards. raised={false} still drops it. L3: list cell card + isTappable = True uses the L3/elevation/soft effect style.
- **1.9.0** (2026-11-03) Label 14 on default rows: Default-size rows can use a Label 14 label and keep their icon slot and height — for stock / ETF card lists (40 logo, symbol and price both Label 14). Stock card lists sit 12 apart. L3: list cell: label text style Label/14 on a default (not isSmall) row for stock card lists (pending as a property in the library). labelSize="16" | "14" on size="md" rows (default 16); renders data-label-size="14".
- **1.8.0** (2026-11-03) Flat tappable cards for long lists: Tappable card rows can drop their shadow for long lists (watchlists, market lists, search results): surface-primary + border-light, no shadow, still presses to 0.98. A few standalone cards stay raised; never mix the two in one list. Stock / ETF row recipe in the usage notes: logo (Avatar 40), symbol, company name on one line, price over a small Price change. Clickable card row without shadow for long lists: L3: list cell card + isTappable with the elevation-low effect removed (pending as a property in the library). raised={false} on variant="card" with onClick / href (default true); renders data-flat.
