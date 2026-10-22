# Generate or redesign screens with the L3 kit

**Agents: for any "design screens / a flow / redesign old screens in Figma" task, read only this file.** It replaces
reading DESIGN_SYSTEM.md, the USAGE files, PLAYBOOK and LEARNINGS for that job — the kit applies those rules for you.
Open another doc only for a question this file doesn't answer, and then only that section (`grep -n`).

Why it works (docs/agent/ALGORITHM.md has the numbers): you write short **L3 JSX** (the React component names and
props), a local compiler checks it against the design rules and fixes what it can, and **one `use_figma` call builds
the whole flow** from real ✅ Lemonnade V3 instances, then checks it and returns snapshots.

---

## 1. The algorithm — follow it in order

| # | Step | Tool | Budget |
|---|---|---|---|
| 0 | **Set up** — repo + this file. `git -C <repo> pull`; `npm ci` once if `node_modules` is missing. | Bash | 0 Figma calls |
| 1 | **Intake.** New flow: list the screens (`"01 Positions"`, `"02 Settings"`…), what each shows, how they connect (push · bottom sheet · toast). Redesign: `npm run kit -- outline <node ids or URLs>` → paste the file it writes as **one** `use_figma` call → a text outline per old screen (§6). | — / 1 call | ≤ 1 call |
| 2 | **Write `flow.jsx`** — every screen of the task in one file, in a work folder **outside the repo** (scratchpad). Content first: real-looking names, ₹ amounts, dates. | Write | 1 write |
| 3 | **Compile** — `npm run kit -- build flow.jsx --parent <page or section id>` (`--parent` overrides `<Flow parent>`; one is enough). Fix every ✖ (edit the JSX), read the ⚠, note the ✓ (already applied). | Bash | until 0 ✖ |
| 4 | **Build** — paste each `flow*.figma.js` it wrote, **whole and unchanged**, as the `code` of one `use_figma` call (in order). | use_figma | 1 call per file (≈ 12–15 screens) |
| 5 | **Review** — the result lists every screen (`id`, `size`, `issues`) plus a 0.5× snapshot of each. Fix by **editing `flow.jsx`**, then rebuild only those screens: `--only "03 Confirm,04 Done"` (rebuilt frames replace the old ones in place). | Edit + 3–4 | ≤ 2 rounds |
| 6 | **Report** — what was built (screen names + ids), auto-fixes worth knowing, open questions (missing components, `PENDING` rules). | — | — |

**Stop and ask** instead of improvising when: something you need isn't in §3 (don't fake a component), the outline
shows a pattern with no L3 equivalent, or you're past the budgets above (something is wrong — say what).

## 2. Cost rules — why the budgets matter

- **Every API call re-reads your whole context.** Cost ≈ context size × number of calls (in our sessions 73% of all
  tokens were these re-reads). So: few calls, small context. One call that builds 12 screens beats 12 calls.
- **Don't explore.** Component keys, variants, slots, text styles, colour/spacing tokens and 190 icons are already in
  the kit. Never `search_design_system` for components, never `get_design_context` / `get_metadata` on whole frames,
  never screenshot old screens (use the outline), never read the kit's source.
- **Images only from the build result** (already 0.5×). Need to look closer? `get_screenshot` one node, `maxDimension` ≤ 800.
- **Long outputs land in files** (Bash > ~30 kB is persisted and must be re-read) — don't print big things.
- **One task per session.** The repo's `.claude/settings.json` compacts at ~250k tokens; start a fresh session for a new flow.
- **Tiny tweak** (one text, one toggle) on a kit-built screen? Still edit `flow.jsx` and rebuild that one screen —
  the JSX stays the source of truth and the kit keeps every rule.

## 3. L3 JSX — the language

The **component elements and props are the React components'** (`src/components`, `docs/figma-code-map.md`). Props take
`"text"`, `{number}`, `{true}`, `{[…]}`, `{{…}}` or `{<Element/>}`; a bare prop is `true`. Comments: `{/* … */}`.
Layout helpers (`Flow Screen Section Stack Row List Text Icon KeyValue Stats Divider Placeholder`) exist only in the kit.

### Structure

| Element | Props | Notes |
|---|---|---|
| `<Flow>` | `name` · `parent` · `width` · `height` · `theme` · `gap` | Optional wrapper: makes / reuses a Section named `name` on the page `parent` and lays screens out in a row. |
| `<Screen>` | `name` (unique) · `width` 360 · `height` 800 · `base` · `theme` · `gap` 24 · `clip` | Top: `Actionbar`. Bottom: `ButtonGroup` **or** `BottomNavbar` **or** `Keyboard`. Everything else is the body (16 top, 24 apart). Taller content grows the frame. `base="02 Settings"` starts from that built screen (for sheets over it). `theme="LM Dark"` / `"CS PRO Dark"` / `"Kuber Light"`. |
| `<Section>` | `title` · `description` · `tag` · `action` (`"View all"` or `{{ type: "switcher", label: "1D" }}`) · `info` · `gap` 16 | A `SectionHeader` + its content, 16 apart. Use for every titled block. |
| `<Stack>` / `<Row>` | `gap` 8 · `pad` (`16` · `"12 16"` · `"page"`) · `align` · `justify` (`between`…) · `wrap` · `bg` · `radius` · `border` | Auto-layout frames bound to spacing tokens. Children of a `Row` hug; give one `grow` to take the rest. |
| `<List>` | `variant` (`card`) · `density` (`compact` · `breathable`) · `gap` | Optional: consecutive `ListCell`s are grouped for you. Asset rows (a `PriceChange`/`Sparkline` trailing) become Breathable. |
| `<Text>` | `style` (`Heading/10–36` · `Label/10–18` · `Description/10–18`) · `color` · `align` · `lines` | Default `Description/14`, primary. Numbers are `Label` (or `Heading` when prominent ≥ 18), never `Description`. |
| `<KeyValue>` | `label` · `value` · `valueStyle` · `change` (number or `{{ value, unit, percent, size }}`) · `align` | Label over value (+ PriceChange). |
| `<Stats>` | `items={[["Invested","₹44,220.50"],…]}` · `columns` 2 · `card` (`false` = no grey card) | Grid of KeyValues in a filled card. |
| `<KeyValueList>` + `<KeyValueRow>` | list: `variant` plain · card · `size` sm · `total={{ label, value }}` — row: `label` · `value` · `caption` · `info` · `copy` · `layout` stacked · `tone` up · down | L3: Key-value list / row. Facts people read (order details, charges, account info, receipts). Stacked for long values (UPI ID, address). Card = static card, no shadow. |
| `<Icon>` | `name` · `size` 24 · `color` | Material name from the Icons library (`npm run kit -- icons <q>`), or a custom icon: `kill_switch` · `auto_tp_sl` · `pnl_protect` · `watchlist_add` · `watchlist_added` (only for that feature). |
| `<Avatar>` | `initials` · `icon` · `size` 24 · 32 · 40 (default) · 48 · `shape` circle · rounded · `src` | L3: Avatar — stock / company logo, fund or person. Type follows the content (icon → Icon, initials → Initials, else the grey Image layer: set the logo as its fill). In rows: `<ListCell iconLeft={<Avatar initials="RI" />}>`; in a card header: first child of a `Row`. |
| `<Tooltip>` | `content` (or children) · `placement` top · bottom · `arrow` start · center · end · `close` | L3: Tooltip — a one-line dark hint (≤ ~40 chars) pointing at an icon or number. `placement="bottom"` = the bubble sits below its target (arrow up). Put it right after the row it points at; `arrow` lines it up (end = under a right-hand icon). `close` shows ✕. |
| `<CoachMark>` | `heading` · `body` · `step` ("1 of 3") · `skip` · `nextLabel` · `skipLabel` · `placement` · `arrow` | L3: Tooltip Type=Coach mark — points out a new feature once. Same placement rules as Tooltip. |
| `<Divider>` / `<Placeholder>` | `inset` / `label` · `height` | Placeholder = grey box for real artwork you don't have (reported). |

Colours (`color`, `bg`, `border`): `primary` `secondary` `tertiary` `disabled` `inverted` `up`/`profit` `down`/`loss`
`success` `error` `warning` `discover` `brand` `zing`, or a token path (`surface/secondary`, `border/light`).
Common per-element props: `name` (layer name), `grow` / `fill` (in a Row), `width`, `height`, `hug`, `bleed`.

### Components (L3 library)

| Element | Key props (defaults first) | The kit also… |
|---|---|---|
| `Actionbar` | `title` · `back` · `description` · `actions={["search", {icon:"settings",label:"F&O settings",variant:"ghost"}]}` (≤ 2; or `<ActionbarAction icon label/>` children) · `bottom={<Tabs appearance="underline" …/>}` · `titleSelect="NIFTY 50"` | L1 (no `back`) → Heading/18 "L1 page heading"; L2 → Heading/14. Actions: Tertiary Small boxed (`variant:"ghost"` only if asked). Underline Tabs placed right under it move into `bottom`. |
| `Button` | children (label) · `variant` primary · secondary · tertiary · ghost · brand · buy · sell · `size` lg/md/sm · `icon`/`iconLeft` · `iconRight` · `disabled` · `loading` · `label` (icon-only name) · `full` | Hugs outside docks (`full` to stretch); size md inside cards/sheets/margins, lg elsewhere; placeholder icons hidden. |
| `ButtonGroup` | `direction` vertical · horizontal · `note` (text above the dock) | The dock. Buttons → lg; strong button (primary/buy/sell/brand) on top / on the right. One per screen. |
| `Tabs` | `items={["A","B"]}` or `[{label, subLabel, icon}]` · `appearance` pill · underline · pill-group · ribbon (special — only when the brief names it; never pick it yourself) · `value` (index or label) · `emphasis` · `size` sm · `behavior` | Pill rows run edge to edge with their own 16 inset — never doubled inside a margin. Only rows that act as tabs fill the width (equal widths if they fit, hug + scroll if not); filters and toggles hug. `behavior` tabs · filter · toggle — defaults: underline = tabs, pill = filter, pill-group = toggle, ribbon = tabs that hug (folder tabs on a dark baseline); set `behavior="tabs"` on a chip row that switches sections. A chip row first in the body moves into the Actionbar (12 top/bottom); inside the content it has 0. |
| `ListCell` | `label` (one line ≤ ~32) · `description` · `size` md/sm · `variant` plain · card · `iconLeft` (icon name or element) · `iconRight` (chevron) · `trailing` (`"switch:on"` · `"switch"` · `"radio:on"` · `"check"` · `"value text"` · `<PriceChange/>` · `[…]`) · `labelSuffix` (side: "Long") · `labelSuffixTone` up · down · neutral · `caption` (second line under `trailing`) · `selected` · `tappable` · `dotLeft`/`dotRight` · `density` | Navigation rows get `chevron_right` (the library default is ⌄). Plain rows inside cards / sheets / margins drop their own 16 inset. |
| `Card` | `variant` static · clickable · flat · filled (or `clickable`) · `padding` (`"none"`) · `selected` · `footer={[<Button>Exit now</Button>]}` · `footerFilled` · `gap` 8 · `direction` (`row`) | Footer buttons Tertiary Small, 12 from the edge. Content 12 from the edge. |
| `SectionHeader` | `title` · `description` · `tag` · `info` · `action` | Prefer `<Section title>`. |
| `Tag` | children · `color` neutral · profit · loss · success · error · warning · discover · processing · zing · indigo · teal · purple · `variant` secondary · primary · tertiary · `size` sm/md/lg · `icon` · `hideLabel` | Never tappable. No green/red/yellow/orange. |
| `PriceChange` | `value` (signed number) · `unit` percent · currency · number · `percent` · `size` sm/md/lg · `arrow` · `decimals` · `text` | Sign + colour from the value; ₹ with Indian grouping. Never colour a change by hand. |
| `TextField` | `label` · `hideLabel` (label stays its name, not drawn — an amount field inside a row) · `placeholder` · `value` · `helperText` · `status` error · success · `disabled` · `required` · `multiline` · `iconLeft`/`iconRight` | |
| `Select` | children · `size` sm/md/lg · `subtle` · `icon` swap · chevron | Inline trigger (opens a sheet of Radio rows). No boxed dropdown exists. |
| `Stepper` | `value` · `size` sm/lg · `sublabel` · `min` · `max` · `label` | |
| `Switch` · `Checkbox` · `Radio` | `checked` · `size` (Switch) · `indeterminate` · `disabled` | In rows use `ListCell trailing="switch:on"`. |
| `Aerobar` | `type` discover · primary · success · warning · danger · `emphasis` · `heading` · `paragraph` · `icon` (name or `false`) · `action` · `tappable` (whole bar/card opens more: chevron on the right, no `action` with it) · `floating` · `placement` bottom · inline | Full-width bar → always placed at the bottom above the dock / navbar (auto-moved). `floating` = toast above the dock/bar with shadow; `floating placement="inline"` = flat card inside the content (a full-width bar written inside content is auto-converted to this). Pick by use case: result / failed+retry → toast; heads-up, action required, system status → full-width bar. |
| `EmptyState` | `title` · `description` · `action` | Only for *nothing to show* — its illustration is always the L3 default empty-state illustration (built in; never pick or draw another). Never for a success / result. |
| `BottomSheet` | `heading` · `description` · `size` sm · lg · `icon` (lg) · `back` · `trailing` · `bottom` · `footer={<ButtonGroup>…</ButtonGroup>}` · `utility` · children (or `header={<BottomSheetHeader heading …/>}`) | Inside a `Screen`: scrim + sheet over it. ≤ 2 per screen; the 2nd gets a back button. No ✕, no handle. Confirmations are sheets (no dialogs). |
| `ProgressBar` | `value` · `type` progress · range · `status` · `size` | |
| `Chart` / `Sparkline` | `type` candle · line · area · `trend` up · down · `ranges={["1D","1W","1M"]}` · `showVolume`… | Charts run edge to edge. |
| `Skeleton` · `SkeletonListRow` · `SkeletonCard` | `shape` | Loading states. |
| `BottomNavbar` | `value` stocks · market · portfolio · mf · fno (· `fno-positions`…) | |
| `BrandLogo` · `Keyboard` · `SystemStatusbar` · `DatePicker` | `brand`, `variant` · `numeric` · `dark` · `mode`, `month` | The Actionbar already includes the status bar. |

**Not in L3** (stop and ask): sliders (dropped — never build one), dialogs, boxed dropdowns, OTP boxes (`OtpInput` is in code; the kit can't place it until the Figma library is published — use `TextField` for now), stock-logo
pictures (`<Avatar>` places the grey Image layer — the real logo is set in Figma), illustrations other than EmptyState's.

## 4. Choosing — the judgment the kit can't make

| Need | Use |
|---|---|
| Screen title + back / actions / top tabs | `Actionbar` (`back` on every non-top-level screen) |
| A titled block of content | `<Section title>` (never a big `Text`) |
| Main action(s) of a screen or sheet | `ButtonGroup` — **one primary per screen**; a Cancel next to it is `secondary` |
| Standalone action ("View all", "Add another") | `Button variant="tertiary"` (in a Section: `action="View all"`) |
| A tappable item that opens something | `ListCell` with `iconRight` (or a clickable `Card` for rich content) |
| On/off setting · pick one · pick many | `ListCell trailing="switch:on"` · `trailing="radio:on"` rows (card rows for options) · `trailing="check:on"` |
| Choose between views (filters, segments) | `Tabs appearance="pill"` (top-of-screen sections: `underline` in the Actionbar). Only rows that switch sections fill the width (`behavior="tabs"`); filters and toggles hug |
| Grouped facts / numbers | `KeyValueList` (label → value rows, optional total); a 2-column number grid → `Stats` |
| Any signed change, P&L, returns | `PriceChange` |
| Status: heads-up · action required · system status → full-width `Aerobar` (bottom); result of an action → `Aerobar floating` (toast); note inside content → `Aerobar floating placement="inline"` |
| Result of a flow (added, placed, submitted, paused…) | `<BottomSheet size="lg" icon="check_circle" iconColor="success" heading description footer={one Button}>` over the last screen — never `EmptyState` (failure: `icon="error"` `iconColor="error"`) |
| A form | `Stack gap={16}` of `TextField`s (`value` filled = typed state, `helperText` for hints / verified names), the submit `Button` in the `ButtonGroup` dock |
| Facts in a confirm / result sheet | `KeyValueList` (or plain `ListCell label trailing="value text"` rows, no `iconRight`) |
| Confirm before acting | `BottomSheet` over the screen: question heading, `ListCell` facts, horizontal `ButtonGroup` (secondary Cancel + strong confirm) |
| Nothing to show / loading | `EmptyState` / `SkeletonListRow` |

**Status messages (Aerobar) — placement, shadow, leaving, by use case:**
- Full-width bar: always at the bottom, just above the dock / navbar (or the screen edge) — never at the top (the kit moves it). Floating at the bottom has a shadow; floating inside the content or near the top is flat (`placement="inline"`).
- Toasts leave on their own (4 s, 6 s with an action). Full-width bars stay until ✕, a button (OK / Dismiss), doing the task, or the state changing.
- Result of an action (Order placed, Copied) → toast, solid success/primary. Failed, can retry → toast, solid danger + Retry (blocking failure → result sheet). Heads-up, time-bound (Market closes in 15 min) → full-width + Dismiss. Action required (KYC pending, add bank, margin shortfall) → full-width, the fix as `action`, no ✕. System status (offline, maintenance) → full-width while true. Note inside content → floating inline.
- One full-width bar (action required → system status → heads-up) and one toast per screen.

**Rows of the same kind:** order / trade history → `ListCell size="sm"` with `labelSuffix` (Long / Buy green, Short / Sell `labelSuffixTone="down"`), a status `Tag` as `trailing` with the time as `caption`; amounts as `trailing` with their type as `caption`.

Content: one-line labels; sentence case; numbers as `₹1,23,456.78`, `+2.40%`; dates `14 Oct 2026`, `3:30 PM`. Write
the copy a real user would see — no lorem ipsum, no "Label".

## 5. Example — paste-ready, compiles clean

```jsx
<Flow name="Price alerts" parent="4070:65339">
  <Screen name="01 Alerts">
    <Actionbar title="Price alerts" back actions={[{ icon: "add", label: "New alert" }]} />
    <Aerobar floating placement="inline" type="discover" heading="Alerts trigger once" paragraph="We notify you when the price crosses your target." />
    <Section title="Active (2)">
      <ListCell label="RELIANCE above ₹3,000" description="Now ₹2,938.55" trailing="switch:on" />
      <ListCell label="NIFTY 50 below 25,000" description="Now 25,312.40" trailing="switch:on" />
    </Section>
    <Section title="Triggered" action="View all">
      <ListCell label="TCS above ₹4,100" description="12 Oct, 10:42 AM" iconRight />
    </Section>
    <ButtonGroup><Button>Create alert</Button></ButtonGroup>
  </Screen>
  <Screen name="02 Delete alert" base="01 Alerts">
    <BottomSheet heading="Delete this alert?" description="RELIANCE above ₹3,000" footer={<ButtonGroup direction="horizontal"><Button variant="secondary">Cancel</Button><Button>Delete</Button></ButtonGroup>} />
  </Screen>
  <Screen name="03 Alert created" base="01 Alerts">
    <BottomSheet size="lg" icon="check_circle" iconColor="success" heading="Alert created" description="We'll notify you when RELIANCE crosses ₹3,000." footer={<ButtonGroup><Button>Done</Button></ButtonGroup>} />
  </Screen>
</Flow>
```

More, from real product screens: Lemonn Portfolio → Orders (cards, chip tabs, navbar), the Tata Motors stock page (chart, holding, SIPs, Sell / Buy dock) and Kuber Orders & History with its order-details sheet and cancel toast — `scripts/figma/kit/examples/real-screens.jsx`.

## 6. Redesigning old screens

1. `npm run kit -- outline <ids or URLs>` → paste `outline.figma.js` as one `use_figma` call (read-only). Each screen
   comes back as text: `⧉ instance props`, `"text"`, `▢ frame ↓gap p t/r/b/l`, repeats `×N`. Old D2 parts keep their names.
2. Map what each block **does** to §4 (not how it looks): a grey stat block → `Stats`; a row of chips → `Tabs pill`; a
   bordered row with a chevron → `ListCell variant="card" iconRight`; an old CTA bar → `ButtonGroup`.
3. Keep the copy and the order; drop decoration the system doesn't have; note anything with no L3 equivalent.
4. Build the redesign next to the old section (`<Flow name="… · L3">`), never over it.
In-place migration that must keep the old layout (swap components, rebind tokens) is a different job: PLAYBOOK §4.

## 7. When something goes wrong

| You see | Do |
|---|---|
| `✖ <X> isn't an L3 element` | Use §3 / §4. Missing for real → stop and ask. |
| `✖ icon "x" isn't in docs/agent/icons.json` | `npm run kit -- icons <part>` for the right name. Not there → one `search_design_system` in "👁️ Lemonnade V3 → Icons", add the key to `icons.json`. |
| `issues: truncated: "…"` | Shorten the label; move detail to `description`. |
| `issues: off-screen` / `placeholder` | Too much in a `Row` (give one child `grow`, drop one) / a prop you left empty. |
| `⚠ X` note drawn in a screen, or `errors` in the result | The kit failed on that element — the rest was built. Fix the JSX; if it's the kit, log it in docs/LEARNINGS.md. |
| `time budget — not built: …` | Re-run with `--only` for those screens. |
| `base "…" not found` | Build the base screen first (same file is fine) or check its name. |

After the task: add one line to `docs/LEARNINGS.md` if something cost you time, and fix the kit (`scripts/figma/kit/`)
or this file so the next agent doesn't pay for it again — then `npm run skill` so the shareable skill (`plugin/`) gets the fix too.
