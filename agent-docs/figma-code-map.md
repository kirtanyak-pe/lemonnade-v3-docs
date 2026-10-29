# Figma → code map

How each component in the Figma library "✅ Lemonnade V3" (`lxQ6QIXGOv5mmx0khh5sJn`) maps to the React components in
`src/components`. Use it to turn a Figma design into code: read the instance's component and properties, then write the
matching JSX. (This stands in for Figma Code Connect, which needs an Organization/Enterprise plan.)

Conventions: Figma `👁️ X` toggles = whether the prop is passed; `✏️ X` text = the prop's string; `↪ X` swaps = the icon
passed (`<Icon icon={ms…} size={16} />`); `Version=❌ Discontinued` variants are never used.

## Layout (auto-layout frames, no Figma component)

Plain auto-layout frames map to the layout components. Read the frame's auto-layout settings; every value is a bound
variable (`spacing/<n>`, `radius/<n>`, `surface/<name>`) and becomes the token suffix.

| Figma frame | React |
|---|---|
| Auto layout, no fill, no padding | `<Stack direction gap align justify wrap>` (`components/Stack`) |
| Direction ↓ / → | `direction="vertical" \| "horizontal"` |
| Gap `spacing/16` | `gap="16"` |
| Alignment (cross axis) left/top · centre · right/bottom · children Fill | `align="start" \| "center" \| "end" \| "stretch"` |
| Packed start · centre · end · Space between | `justify="start" \| "center" \| "end" \| "between"` |
| Wrap | `wrap` |
| Auto layout ↓ with padding and/or a fill, radius, stroke | `<Section padding gap align surface radius border>` (`components/Section`) |
| Padding all `spacing/12` · H / V different | `padding="12"` · `paddingInline="16" paddingBlock="08"` |
| Fill `surface/secondary` · none | `surface="secondary"` · `surface="none"` |
| Corner radius `radius/12` · Stroke `border/light` 1 | `radius="12"` · `border` |
| Frame with padding only (page gutter 16 / 0) | `<Inset paddingInline="16" paddingBlock="00">` (`components/Inset`) |
| Auto layout Wrap, equal fixed-width children, N per row | `<Grid columns={N} gap align>` (`components/Grid`) |

## Actions

### L3: Button → `<Button>` (`components/Button`)
| Figma | React |
|---|---|
| Type = ◻️ Primary · 🔲 Secondary · ⬜︎ Tertiary · Ghost · 🟨 Brand · 🟩 Buy · 🟥 Sell | `variant="primary" \| "secondary" \| "tertiary" \| "ghost" \| "brand" \| "buy" \| "sell"` |
| Ghost instance set to Hug (width + height) | `variant="ghost"` — code always hugs. Figma's Ghost variants stay fixed size (existing designs rely on it), so hug each new instance where you place it, like the Section header CTA. |
| Size = Large · Medium · Small | `size="lg" \| "md" \| "sm"` |
| State = ♻︎ Loading | `loading` |
| State = 🚫 Disabled | `disabled` |
| ✏️ Label (👁️ Label on) | children |
| 👁️ Label off (icon-only) | no children, one icon, `aria-label` required |
| ↪ Icon-L / ↪ Icon-R (👁️ on) | `iconLeft` / `iconRight` |

### L3: Button Dock → `<ButtonGroup>` (`components/ButtonGroup`)
| Figma | React |
|---|---|
| Direction = ↓ Vertical · → Horizontal | `direction="vertical" \| "horizontal"` |
| Scroll indicator | `scrollIndicator` |
| wrapper (slot) | `<Button size="lg">` children; `aria-label` required |

## Input & control

### L3: input field & text Box → `<TextField>` (`components/TextField`)
| Figma | React |
|---|---|
| ✏️ Label (👁️ Label) | `label` (always visible in practice) |
| 👁️ Required | `required` |
| ✏️ Placeholder text | `placeholder` |
| ✏️ Input text | `value` / `defaultValue` |
| ✏️ Helper text (Description) | `helperText` (Description-Icon → `helperIcon`) |
| State = Error · Success | `status="error" \| "success"` |
| State = Disabled | `disabled` |
| State = Default · Typing · Typed | runtime states — no prop |
| isInputBox = True | `multiline` (+ `maxLength` for the counter) |
| ↪ Icon-L / ↪ Icon-R | `iconLeft` / `iconRight` |

### L3: Tooltip → `<Tooltip>` / `<CoachMark>` (`components/Tooltip`)
| Figma | React |
|---|---|
| Type = Tooltip (✏️ Text) | `<Tooltip content="…">target</Tooltip>` |
| 👁️ Close | `onClose` (shows ✕, stays open until tapped) |
| Type = Coach mark (✏️ Heading · ✏️ Body) | `<CoachMark open heading body onNext>target</CoachMark>` |
| ✏️ Step (👁️ Step) · 👁️ Skip | `step="1 of 3"` · `onSkip` |
| Placement = Top · Bottom | `placement="top" \| "bottom"` (flips to fit) |
| Arrow = Start · Center · End | automatic — the arrow follows the target |

### L3: Key-value list → `<KeyValueList>` (`components/KeyValueList`)
| Figma | React |
|---|---|
| isCard = False · True | `variant="plain" \| "card"` |
| Size = Medium · Small | `size="md" \| "sm"` (rows inherit) |
| Rows (slot) | `<KeyValueRow>` children |
| 👁️ Total (divider + isTotal row) | `total={{ label, value, caption? }}` |

### L3: Key-value row → `<KeyValueRow>` (`components/KeyValueList`)
| Figma | React |
|---|---|
| ✏️ Label · ✏️ Value | `label` · `value` |
| Layout = Horizontal · Stacked | `layout="horizontal" \| "stacked"` |
| isTotal = True | `total` (set by the list's `total`) |
| 👁️ Info | `info` (tooltip text) or `onInfo` |
| 👁️ Icon-R (content_copy) | `copy` |
| 👁️ Icon-R + ↪ Icon-R (other icon) | `valueEnd={<Button … />}` |
| 👁️ Caption + ✏️ Caption | `caption` |
| (value fill → indicator up / down) | `tone="up" \| "down"` |

### L3: Badge → `<Badge>` (`components/Badge`)
| Figma | React |
|---|---|
| Type = Count (✏️ Count) | `count` (over `max` → "99+", 0 hides it) |
| Type = Dot | `dot` |
| Color = Error · Discover · Neutral | `color="error" \| "discover" \| "neutral"` |
| Size = Small · Medium | `size="sm" \| "md"` |
| (on an icon, top-right) | `<Badge …><Icon /></Badge>` · BottomNavbar / Tabs item `badge` |

### L3: Loader → `<Loader>` (`components/Loader`)
| Figma | React |
|---|---|
| Size = Small (16) · Large (96) | `size="sm" \| "lg"` |
| (text next to it, e.g. “Verifying…”) | `label={null}` + `role="status"` on the text's element; otherwise `label="…"` (screen-reader only) |

### L3: Stage bar → `<StageBar>` (`components/StageBar`)
| Figma | React |
|---|---|
| Stages = 2 · 3 · 4 · 5 | `stages={2 \| 3 \| 4 \| 5}` |
| Current = 1–5 | `current={n}` |
| ✏️ Stage (👁️ Stage) | `stage="…"` (leave out = hidden) |
| "Step n of n" text | built in; `stepText` to replace it |
| L3: base Stage segment · State | automatic from `current` (before = done, current, after = to do) |

### L3: Steps → `<Steps>` (`components/Steps`)
| Figma | React |
|---|---|
| L3: Steps · Steps slot (rows top to bottom) | `steps={[…]}` + `label` (screen readers) |
| L3: base Step · ✏️ Label | `steps[i].label` |
| ✏️ Description (👁️ Description) | `steps[i].description` (leave out = hidden) |
| State = Done · Current · Loading · To do | `state: 'done' \| 'current' \| 'loading' \| 'todo'` |
| isLast = True | automatic (the last item) |

### DASH AI parts → AI chat (`components/Ai`)
No L3 library components yet — these map the DASH AI flow (11491:7726) frames to code.

| Figma | React |
|---|---|
| Dash entry point (11492:7740) | `<AiEntryPill icon label ariaLabel onClick>` — the orb goes in `icon` |
| Gradient + Layer_1 (screen background) | `<AiGlow position="header" \| "center" \| "bottom" tone="brand" \| "negative">` |
| ✦ HERE ARE SOME EXAMPLES ✦ | `<AiSectionLabel>` |
| Example prompt chips (rows) | `<AiSuggestionChips rows label onPick>` |
| Input field + Send Button | `<AiPromptInput label placeholder onSend>` |
| User message (grey pill) · DASH reply (title + text) | `<AiMessage role="user">` · `<AiMessage role="assistant" title>` |
| Thinking (dots + text) | `<AiThinking>` |
| Quick replies (green-edged = suggested) | `<AiReplyChips options={[{ label, suggested }]}>` |
| Question card (5612:40650) | `<AiChoiceCard question options onAnswer freeText onClose>` |
| Rule card (5612:41008) · value chip / description | `<AiDraftCard icon title value>` · `<AiDraftCard … description onOpen insight>` |
| Backtest strip · read-more pill | `<AiInsight tone="positive" \| "negative" \| "neutral">` · `shape="card"` |

### L3: Avatar → `<Avatar>` (`components/Avatar`)
| Figma | React |
|---|---|
| Type = Image (fill the "Image" layer) | `src` (falls back to initials, then icon) |
| Type = Initials (✏️ Initials) | `initials` |
| Type = Icon (↪ Icon) | `icon` (default person) |
| Size = 24 · 32 · 40 · 48 | `size={24 \| 32 \| 40 \| 48}` |
| Shape = Circle · Rounded | `shape="circle" \| "rounded"` |
| (in List cell Icon-L slot) | `<ListCell iconLeft={<Avatar … />}>` — keeps its size |

### L3: OTP input → `<OtpInput>` (`components/OtpInput`)
| Figma | React |
|---|---|
| Length = 4 · 6 | `length={4 \| 6}` |
| State = Error · Success | `status="error" \| "success"` |
| State = Disabled | `disabled` |
| State = Default · Typing | runtime (focus / typed digits) — no prop |
| ✏️ Label (👁️ Label) | `label` |
| Helper text (👁️ Helper text) | `helperText` |
| 👁️ Edit (✎ after the helper text) | `edit={{ label: 'Edit phone number', onClick }}` |
| 👁️ Resend | `action={{ label: 'Resend', onClick }}` |
| L3 base: OTP digit ✏️ Digit | `value` / `defaultValue` (one digit per box) |

### L3→ Toggle switch → `<Switch>` (`components/Switch`)
| Figma | React |
|---|---|
| ↔ On = True · False | `checked` / `defaultChecked` |
| isSmall = True | `size="sm"` (default `md`) |
| isTappable = True · False | `onClick` / `href` / `as="label"` · none — card rows: Clickable (surface/primary + elevation-low) · Static (no fill + border/light) |
| isSelected = True (tappable only) | `selected` — plain: surface/secondary · card: border/dark |
| (no Figma variant) | `disabled` |

### L3: Radio button & check box → `<Checkbox>` / `<Radio>` (`components/Checkbox`)
| Figma | React |
|---|---|
| isRadio = False · True | `<Checkbox>` · `<Radio>` (radios share a `name`) |
| 👆 State = selected | `checked` |
| 👆 State = Intermediate | `indeterminate` (Checkbox only) |
| Disabled = True | `disabled` |

### L3: Stepper → `<Stepper>` (`components/Stepper`)
| Figma | React |
|---|---|
| Size = Small · Large | `size="sm" \| "lg"` |
| ✏️ Value | `value` (+ `format`) |
| ✏️ Sublabel / 👁️ Sublabel | `sublabel` (Large only) |
| .L3: Stepper button State = Disabled | automatic at `min` / `max` (or `disabled`) |
| State = Default · Focused | automatic: Focused while the number is being typed (`editable`, default true) |

### L3: Select switcher → `<Select>` (`components/Select`)
| Figma | React |
|---|---|
| ✏️ Label | children |
| Size = Small · Medium · Large | `size="sm" \| "md" \| "lg"` |
| isSubtle | `subtle` |
| ↪ Icon (D2 → Switch arrow toggle ↕ · expand_more) | `icon="swap" \| "chevron"` (↕ = `lmSwitchArrowVertical` from `icons/lemonnade`) |

**Custom icons** (not Material; Icons library → "Custom icons" frame), all from `src/icons/lemonnade` with `<Icon icon={…}>`: `D2 → kill_switch` → `lmKillSwitch` · `D2 → auto_tp_sl` → `lmAutoTpSl` · `D2 → pnl_protect` → `lmPnlProtect` · `D2 → Switch arrow toggle` Direction=↕ / ↔ → `lmSwitchArrowVertical` / `lmSwitchArrowHorizontal`.

### L3: Date picker → `<DatePicker>` (`components/DatePicker`)
| Figma | React |
|---|---|
| Mode = Single · Range | `mode="single" \| "range"` |
| ✏️ Month | from `initialMonth` / the selection (navigates itself) |
| .L3: Date cell State = Disabled | days outside `min` / `max` |
| State = Today · Selected · Range start/middle/end · Empty | automatic |

## Navigation

### L3: Tabs group → `<Tabs>` (`components/Tabs`)
| Figma | React |
|---|---|
| Type = Flat tabs · Pill tabs · Pill group · Ribbon | `appearance="underline" \| "pill" \| "pill-group" \| "ribbon"` |
| wrapper (slot) of base tabs | `items={[{ value, label, … }]}`, `value`, `onChange`, `aria-label` |

### L3: base tab → one item in `Tabs` `items` (or `<Tab>` when composing)
| Figma | React |
|---|---|
| Style = Flat · Pill · Ribbon | `appearance="underline" \| "pill" \| "ribbon"` (set on `Tabs`; Ribbon is Primary only and a special case — only when the design specifies it) |
| Type = Primary · Secondary · Tertiary (pills) | `emphasis="primary" \| "secondary" \| "tertiary"` (set on `Tabs`; one style per row) |
| isSmall = True | `size="sm"` (on `Tabs`) |
| isSelected = True | that item's `value` is the `Tabs` `value` |
| ✏️ label | item `label` |
| 👁️ Label off | item `hideLabel: true` (+ one icon) |
| ✏️ Sub label (👁️ Sub label) | item `subLabel` (pills only) |
| ↪ icon - l / ↪ Icon - r | item `iconLeft` / `iconRight` |

### L3: Actionbar → `<Actionbar>` (`components/Actionbar`)
| Figma | React |
|---|---|
| 👁️ Action - left (back) | `onBack` |
| Base actionbar content: Heading / Description | `title` / `description` — title Heading/18 without a back button (L1; Figma: an "L1 page heading" text in the slot), Heading/14 with one (L2) |
| Base actionbar content: Type = Search · Searched | `search={{ value, onChange, placeholder }}` |
| → content right (slot) | `actions` — 1–2 `<ActionbarAction icon label onClick>`: Tertiary Small (boxed) by default; `variant="ghost"` when the design removes the box |
| ↓ Content bottom (slot) | `bottom` — e.g. `<Tabs>` |
| Scrolled / elevated look | `sticky` or `elevated` |

### L3: Bottom Navbar → `<BottomNavbar>` (`components/BottomNavbar`)
| Figma | React |
|---|---|
| Tab = Stocks · Market · Portfolio · MF · F&O | `items` = main nav, `value` = selected item |
| Tab = MF - Funds · MF - Dashboard · MF - SIPs | MF sub-nav `items` + `home` |
| Tab = F&O - Option Chain · F&O - Positions · F&O - Scalper | F&O sub-nav `items` + `home` |
| L3 → base navoption (internal) | one entry in `items` |

### Side nav → `<SideNav>` (`components/SideNav`) — code only, not in Figma yet
| Design | React |
|---|---|
| Column of square icon-over-label tiles beside a panel (tools sheet: Range · Indicators · Drawings · Settings) | `items` (`{ value, label, icon, flag? }`), `value`, `onChange`, `aria-label` |
| Selected tile filled grey / blue tint | `selectedTone="neutral"` (default) / `"discover"` |
| "NEW" corner flag (blue / Zing yellow) | item `flag="NEW"`, `flagTone="discover"` (default) / `"zing"` |

## Surfaces

### L3: Bottom sheet → `<BottomSheet>` (`components/BottomSheet`)
| Figma | React |
|---|---|
| isBottom = True · False | `placement="bottom" \| "top"` |
| 👁️ Header | `header={<BottomSheetHeader …/>}` |
| content slot | children |
| Utility slot | `utility` |
| (dock under the content) | `footer={<ButtonGroup>…</ButtonGroup>}` |
| L3: Overlay | built in (the backdrop) |
| — | `open`, `onClose`, and a name (`aria-label` / `aria-labelledby`) |

### L3: Bottom sheet header → `<BottomSheetHeader>`
| Figma | React |
|---|---|
| ✏️ Heading | `heading` |
| ✏️ Description (👁️) | `description` |
| isSmall = True · False | `size="sm" \| "lg"` |
| 👁️ Back button | `onBack` (second, stacked sheet only) |
| H-Icon (👁️ H-Icon) | `icon` |
| 👁️ header tag | `tag={<Tag …/>}` |
| 👁️ info | `info` (+ `onInfo`) |
| right slot (👁️ Action - right) | `trailing` |
| Content bottom (👁️) | `bottom` — tabs or search |

### L3: Card → `<Card>` (`components/Card`)
| Figma | React |
|---|---|
| Type = Clickable | `onClick` or `href` (the whole card is the tap target) |
| Type = Static | neither `onClick` nor `href` |
| Type = Flat | `variant="flat"` (+ `onClick`/`href` if it's clickable) |
| Type = Filled | `variant="filled"` (grey inset panel) |
| isPadded = True · False (False: Clickable, Static, Flat — content brings its own 12) | `padding="default" \| "none"` (not with `variant="filled"`) |
| isSelected = True (Clickable · Flat) | `selected` — Clickable: border/dark · Flat: surface/secondary |
| content (slot) | children |
| action buttons at the bottom of the card (no fill) | `footer` |
| …as a full-width grey strip (intentional emphasis) | `footer` + `footerFilled` |
| (manual background) | `surface` |

### L3: Section header → `<SectionHeader>` (`components/SectionHeader`)
| Figma | React |
|---|---|
| ✏️ Heading | `title` (required) |
| ✏️ Description (👁️ Description) | `description` — one line ideally, max two |
| 👁️ Tag | `tag` (`<Tag size="sm">`) |
| 👁️ Info | `onInfo` (+ `infoLabel`) |
| 👁️ CTA off | no `action` |
| 👁️ CTA on — nested "L3 base: section header cta" Type = Button · Time Switcher | `action={{ type: 'view-all', onClick }}` (Ghost button hugging its label, 16 tall) · `action={{ type: 'switcher', label, onClick }}` (L3: Select switcher, Small) |
| touch area (48) | built in (≥ 48 × 48) |

## Feedback & status

### L3: aerobar - toast → `<Aerobar>` (`components/Aerobar`)
| Figma | React |
|---|---|
| Type = Primary · Discover · Danger · Success · Warning | `type="primary" \| "discover" \| "danger" \| "success" \| "warning"` |
| isPrimary = True · False | `emphasis="primary" \| "secondary"` (solid · light tint) |
| isFloating = True | `floating` (toast) |
| Headline text (👁️ Heading) | `heading` |
| Paragraph text (👁️ Paragraph) | `paragraph` |
| icon-L (👁️ Icon-L) | `icon` (`false` hides it) |
| 👁️ Action-r | `action` |
| 👁️ Chevron-r (tappable) | `onClick` or `href` — the whole surface is the tap target; no `action` / ✕ with it |

### L3 → Empty state → `<EmptyState>` (`components/EmptyState`)
| Figma | React |
|---|---|
| ✏️ Heading | `title` |
| ✏️ Description | `description` |
| Illustration (slot) | `illustration` — default `<EmptyStateIllustration />` = Figma "L3: Empty state default illustration" (5599:7131). Keep it; `null` hides it |
| (button under it) | `action` |

### L3: Skeleton → `<Skeleton>` (`components/Skeleton`)
| Figma | React |
|---|---|
| Shape = Line · Circle · Box | `shape="line" \| "circle" \| "box"` (+ `width`, `height`) |
| isOnGrey | `onGrey` |
| L3: Skeleton pattern Type = List row · Card | `<SkeletonListRow />` · `<SkeletonCard />` |

### L3: Progress bar → `<ProgressBar>` (`components/ProgressBar`)
| Figma | React |
|---|---|
| Type = Progress · Range | `type="progress" \| "range"` |
| Size = Small · Medium | `size="sm" \| "md"` |
| Status = Default · Success · Warning · Error | `status` |
| Value (nested fill / marker, steps of 10) | `value` (any 0–100) |

## Data display

### L3: list cell → `<ListCell>` (`components/ListCell`)
| Figma | React |
|---|---|
| isPlain = True · False | `variant="plain" \| "card"` |
| isSmall = True | `size="sm"` (default `md`) |
| ✏️ Label · ✏️ Description | `label` · `description` |
| isMultiline = True | `multiline` (description wraps; default one line + "…") |
| 📐 L3 → Density mode = Compact · Breathable (set on the list frame or a row) | `density="compact" \| "breathable"` — flat rows only (both sizes); Breathable for asset lists; card rows never change |
| Icon-l (👁️ Icon - L) | `iconLeft` |
| icon-r (👁️ Icon - R) | `iconRight` (chevron) or `trailing` (switch, tag, value) |
| 👁️ Dot-L · 👁️ Dot-R | `dotLeft` · `dotRight` (+ `dotLabel`) |
| 👁️ / ✏️ Label suffix (coloured, after the label) | `labelSuffix` + `labelSuffixTone="up" \| "down" \| "neutral"` |
| 👁️ Trailing → Trailing slot (value / Tag / Price change) + ✏️ / 👁️ Caption | `trailing` + `caption` (stacked, right-aligned; text trailing = the value) |
| (tappable row) | `as="button"` + `onClick`, `href`, or `as="label"` with a Switch/Checkbox |

### L3: Tags → `<Tag>` (`components/Tag`)
| Figma | React |
|---|---|
| Type = Primary · Secondary · Tertiory | `variant="primary" \| "secondary" \| "tertiary"` |
| Type = Disabled | `disabled` |
| Color = Neutral · 🟩 Profit · 🟥 Loss · ✅ Success · 🚨 Error · ⚠️ Warning · 🔷 Discover · 🟠 Processing · indigo · teal · purple · ⚡ Zing | `color="neutral" \| "profit" \| "loss" \| "success" \| "error" \| "warning" \| "discover" \| "processing" \| "indigo" \| "teal" \| "purple" \| "zing"` |
| Size = Small → 16 · Medium → 20 · Large → 24 | `size="sm" \| "md" \| "lg"` |
| ✏️ Label | children |
| 👁️ Label off | `hideLabel` (+ one icon; label text still required) |
| ↪ Icon-L / ↪ Icon-R | `iconLeft` / `iconRight` |

### L3: Price change → `<PriceChange>` (`components/PriceChange`)
| Figma | React |
|---|---|
| Direction = Up · Down · Flat | the sign of `value` |
| ✏️ Value (number without sign) | `value` (+ `unit`, `percent`, `decimals`) |
| Size = Small · Medium · Large | `size="sm" \| "md" \| "lg"` |
| 👁️ Arrow | `arrow` |

### L3: Chart → `<Chart>` (`components/Chart`)
| Figma | React |
|---|---|
| Type = Candle · Line · Area | `type="candle" \| "line" \| "area"` |
| Trend = Up · Down | `trend` (defaults to first vs last close) |
| 👁️ Volume · Grid · Axes · Last price | `showVolume` · `showGrid` · `showAxes` · `showLastPrice` |
| ✏️ Last price | the last `close` (via `format`) |
| (sample data) | `data` |

### L3: Sparkline → `<Sparkline>` (`components/Chart`)
| Figma | React |
|---|---|
| Trend = Up · Down | `trend` (defaults to first vs last value) |

## Brand & device

### L3 → Brand logo → `<BrandLogo>` (`components/BrandLogo`)
| Figma | React |
|---|---|
| Brand = 🍋 Lemonn · ⭐ Zing | `brand="lemonn" \| "zing"` |
| isFull = True · False | `variant="full" \| "icon"` |

### L3: System statusbar → `<SystemStatusbar>` (mockups only)
| Figma | React |
|---|---|
| isDark = True | `inverted` |

### L3: Overlay → `<Overlay>` (`components/Overlay`)
| Figma | React |
|---|---|
| container slot | render the modal panel after `<Overlay>` in the same positioned container |
| (shown) | `open` · tap = `onClick` |

## Variable modes (themes)

Set on `<html>` or any wrapper (DESIGN_SYSTEM 13).

| Figma | React / CSS |
|---|---|
| 🎨 L3 → Theme = 🍋 LM · CS PRO · 🐲 Kuber → Light · Dark | `data-product="lm" \| "cspro" \| "kuber"` + `data-mode="light" \| "dark"` |
| ♿ Accessible … modes | `data-contrast="accessible"` |
| 🌌 Number = Mode 1 | default, no attribute |
| 🌌 Number = Tile (sharp corners) (test) | `data-shape="tile"` (`useTheme().setShape('tile')`): every `--l3-radius-*` is 0 |

## Not in code

| Figma | Status |
|---|---|
| L3: Title | Figma-only — not built in code (by decision) |
| L3: System navbar · L3: System keyboard | Device chrome for mockups — not app UI |
| L3: Space block · L3: Utility / Component container · L3: dev-note · L3: component slot | Figma-only utilities — use CSS gaps / layout in code |
| L3 → State layer | Built into each component (`--l3-state-layer-*`) |
