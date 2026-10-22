# OTP input

> OTP input takes a one-time code — 4 or 6 digits, one per box — for logins, verifying a number or bank account, and confirming risky actions.

- Group: Input & control
- Lifecycle: done
- Status: Figma synced
- Version: 1.1.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5475-547
- Source: `src/components/OtpInput`
- Also called: One-time password, verification code, PIN, passcode, 2FA code

## Import

```tsx
import { OtpInput } from './components/OtpInput' // path relative to src/
```

## Overview

### Typing, pasting and autofill

Each box takes one digit and jumps to the next. Backspace goes back a box, and pasting a code — or the phone filling it from the SMS — fills every box at once. When the last box is filled the code is checked straight away, so a Verify button is optional.

Each box takes one digit and focuses the next; Backspace clears and moves back; ← → Home End move. Pasting, or SMS autofill (`autocomplete="one-time-code"` on the first box), fills from the focused box. `onComplete` fires once every box is filled.

### States

States match the input field. Default and Typing happen on their own: the focused box gets a dark border and a blue caret. Error turns every box red and says what went wrong and how many tries are left. Success says “Verified” in green. Keep Resend in the helper row, disabled while its timer runs, and put a small ✎ right after the phone number so people can fix a wrong number without going back.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `length` | `4 \| 6` | `6` | Figma Length: number of boxes. |
| `value / onChange` | `string / (code) => void` |  | The whole code (or use defaultValue). |
| `onComplete` | `(code) => void` |  | Fires when every box is filled — verify here. |
| `label` | `string` |  | Figma ✏️ Label; also names the group for screen readers. |
| `helperText` | `ReactNode` |  | Figma helper text row (instructions, timer, error, Verified). |
| `status` | `'error' \| 'success'` |  | Figma State=Error / Success. |
| `edit` | `{ label, onClick }` |  | Figma 👁️ Edit: a ✎ icon button right after the helper text (e.g. “Edit phone number”). Hidden while status is set. |
| `action` | `{ label, onClick, disabled? }` |  | Figma 👁️ Resend: a small Ghost button in the helper row. |
| `mask` | `boolean` | `false` | Show digits as •. |
| `disabled` | `boolean` | `false` | Figma State=Disabled. |
| `name` | `string` |  | Submits the code as one form value. |

## Tokens used

- `surface/primary · disabled`
- `border/light · dark · accent/error-default`
- `content/primary · secondary · disabled`
- `content/accent/error-default · success-default · discover-default`
- `Label/12 · 18`
- `Description/12`
- `size/48`
- `radius/12`
- `shadow/elevation-low`
- `spacing/04 · 08`

## Recent changes

- **1.1.0** (2026-10-11) Edit the phone number: A small ✎ button right after the helper text lets people change the number the code was sent to. It shows in Default, Typing and Disabled, and hides with Error or Success. L3: OTP input has 👁️ Edit: an icon-only Ghost Small button with the edit icon after the helper text.
- **1.0.0** (2026-10-11) New: OTP input: OTP input for one-time codes: 4 or 6 boxes, label, helper text and a Resend button. States match the input field — Default, Typing, Error, Success, Disabled. Typing moves to the next box, Backspace goes back, and pasting or SMS autofill fills every box. Digits can be masked. New Figma components: L3: OTP input (Length × State) and L3 base: OTP digit, on the Input field page.
