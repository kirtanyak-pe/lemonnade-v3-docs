# Aerobar & toast

> A short status message with an optional action — inline as a full-width bar, or floating as a toast after something happens.

- Group: Feedback & status
- Lifecycle: done
- Status: Figma synced
- Version: 1.7.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=4543-65562
- Source: `src/components/Aerobar`
- Also called: Toast, snackbar, banner, alert bar, notification

## Import

```tsx
import { Aerobar } from './components/Aerobar' // path relative to src/
```

## Overview

### Toasts

Floating, solid toasts for results people should notice — always shown here; in the phone above they rise in after Buy or Sell.

### Tappable

When a bar or card opens more — details, a sheet, the related screen — the whole surface is the tap target and a chevron shows on the right (Figma 👁️ Chevron-r). Turn the action off: never a button and a chevron together.

### Where it goes

- **Full-width bar: always at the bottom.** It sits just above the button dock or bottom navbar — or at the bottom of the screen if there's neither. Never stick it to the top of the screen.
- **Floating at the bottom: with shadow.** A toast floats 16 above whatever is at the bottom (dock, navbar or a full-width bar).
- **Floating anywhere else: no shadow.** If a message has to sit inside the content or near the top, use the floating card flat — no shadow, inside the 16 page margin.
- **One at a time.** At most one full-width bar and one toast on screen. A new toast replaces the old one. If two bars compete, show the most important: action required → system status → heads-up.

`floating` + `placement="bottom"` (default) = shadow; `placement="inline"` = flat, no inset of its own.

### How it leaves

- **Floating toasts leave on their own** — after 4 s, or 6 s if they have an action. The timer pauses while it's hovered or focused. Never put something people must act on in a toast.
- **Full-width bars stay.** Depending on the use case they are closed with ✕, with a button (OK, Dismiss), by doing what they ask (Complete KYC), or they go away by themselves once the situation changes (KYC done, back online).

Pass `onDismiss`: on a full-width bar it shows a ✕ (unless `action` already closes it); on a toast it's called when the timer ends (`duration` to change it). Leave `onDismiss` out for bars that only go when the state changes.

### Pick by use case

- **Result of an action** — Order placed, Alert set, Copied. Floating at the bottom, solid success / primary. Leaves on its own; optional View or Undo.
- **Action failed, can retry** — Couldn't refresh prices, Order rejected. Floating at the bottom, solid danger, with Retry. Leaves on its own. If the failure blocks the flow, use a bottom sheet instead.
- **Heads-up, time-bound** — Market closes in 15 min, Price alert expires today. Full-width, warning or discover. Closed with ✕ or Dismiss, and goes away when the time passes.
- **Action required** — KYC pending, Add a bank account, Margin shortfall, Nominee missing. Full-width, warning or danger, with the fix as the action (Complete KYC). No ✕ — it stays until the task is done.
- **System status** — You're offline, Exchange maintenance tonight, Market closed. Full-width, primary or warning. Stays while it's true; planned notices can add ✕.
- **Note inside the content** — Alerts trigger once, a tip above a form section. Floating, flat (no shadow), placed where it applies. Static, or ✕ if it's a one-time tip.

### Soft or solid

Figma's isPrimary: the light tint (False) suits information that can wait; the solid color (True) is for results people should notice straight away. Danger is read out to screen readers immediately; the others politely.

Figma's isPrimary: the default light tint suits information that can wait; the solid color (emphasis="primary") is for results people should notice straight away. Danger is announced immediately to screen readers (role="alert"); the rest politely (role="status"). For a danger bar that is part of the page rather than a new event, pass `role="status"` so it isn't read out as an alert on every visit.

## Do / Don't

### Toasts for results, bars for what needs doing

- ✅ **Do:** Put tasks people must finish in a full-width bar at the bottom that stays until it's done.
- ❌ **Don't:** Put a task in a toast — toasts leave on their own after a few seconds.

### Bars live at the bottom

- ✅ **Do:** Place the full-width bar just above the dock or navbar. Inside the content, use the floating card without shadow.
- ❌ **Don't:** Stick a full-width bar to the top of the screen under the title.

## Options (tree)

Aerobar & toast — Status on the page, or a result

- **Type** — Status accents
  - `success`
  - `danger` — Announced as an alert.
  - `warning`
  - `discover`
  - `primary` — Neutral.
- **Emphasis**
  - `primary (solid)` — Results that need attention.
  - `secondary (soft)` — Quieter status on the page.
- **Placement**
  - `inline` — A message that belongs to the page.
  - `floating (toast)` — The result of an action. With an action it stays until tapped.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `type` | `'primary' \| 'discover' \| 'danger' \| 'success' \| 'warning'` | `'primary'` | Figma Type. |
| `emphasis` | `'primary' \| 'secondary'` | `'secondary'` | Figma isPrimary: solid color (primary) or light tint (secondary). |
| `floating` | `boolean` | `false` | Figma isFloating: toast card with a rise-in animation. Without it, a full-width bar that always sits at the bottom above the dock / navbar. |
| `placement` | `'bottom' \| 'inline'` | `'bottom'` | Floating only. bottom: above the dock with a shadow · inline: inside the content or near the top, flat (no shadow). |
| `onDismiss` | `() => void` |  | Full-width: shows a ✕ (unless action closes it). Floating: called when the timer ends. Leave out for bars that stay until the state changes. |
| `duration` | `number` | `4000 · 6000 with action` | Floating only: ms before onDismiss; paused on hover and focus. |
| `heading / paragraph` | `ReactNode` |  | Figma Headline text / Paragraph text (hidden when not passed). |
| `icon` | `ReactNode \| false` | `info icon` | Figma icon-L slot (24px); false hides it. |
| `action` | `{ label, onClick }` |  | Figma Action-r: small Ghost button in a 48px slot; its label follows the bar color. |
| `onClick` | `(event) => void` |  | Figma 👁️ Chevron-r: the whole bar / card is tappable (a button) with a chevron on the right in the heading color. No action or ✕ with it. |
| `href` | `string` |  | Like onClick, as a link. |

## Tokens used

- `surface/tertiary · inverted`
- `surface/accent/* (light · default)`
- `content/primary · secondary · inverted`
- `static/white · black`
- `opacity/60 · 80`
- `Label/14`
- `Description/12`
- `radius/12`
- `shadow/elevation-low · medium`
- `motion/* (local)`

## Recent changes

- **1.7.0** (2026-10-14) Tappable status card: Tappable: when a bar or card opens more (details, a sheet, the related screen), the whole surface is the tap target with a chevron on the right in the heading color — hover and press feedback, and floating cards press like clickable cards. No action button or ✕ with it. onClick / href make it a button / link (the heading and paragraph stay its name). L3: aerobar - toast: new 👁️ Chevron-r on all 20 variants (chevron_right, 20px, takes the heading color). Turn 👁️ Action-r off with it.
- **1.6.0** (2026-10-11) Placement, shadow and dismiss rules by use case: Full-width bars always sit at the bottom, just above the button dock or bottom navbar (or the screen edge) — never at the top. Floating inside the content or near the top is flat (no shadow); floating at the bottom keeps its shadow. Toasts leave on their own (4 s, 6 s with an action, paused on hover/focus). Full-width bars stay: ✕, a button, the fix itself, or a change of state removes them. Six use-case groups: result, failed + retry, heads-up, action required, system status, note in content. placement="bottom" | "inline", onDismiss (✕ on full-width bars, timer on toasts), duration. Floating inside the content: remove the drop shadow on the instance (effect style → none).
- **1.5.0** (2026-10-11) Figma sync: borders, smaller icon, even padding: Soft bars and toasts have a 1px border: border-intense on the neutral one, the matching light accent border (discover, error, success, warning) on the others. Solid ones have none. The icon is 20px (was 24). Toasts pad 12 on both sides (was 6 on the right) and every toast floats on elevation-medium — the soft neutral one was on low. The action button hugs its label instead of sitting in a 48px slot; its 48 × 48 touch area is unchanged. Inline bars stay flat. Figma gives them elevation-medium too, but page messages aren't lifted (Layering rules) — flagged for Figma.
