# Badge

> A badge is a small count or dot on an icon, tab or row — unread notifications, open orders, something new.

- Group: Data display
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5483-93
- Source: `src/components/Badge`
- Also called: Counter, notification count, unread dot, indicator, pill count

## Import

```tsx
import { Badge } from './components/Badge' // path relative to src/
```

## Overview

### Count or dot

Use a count when the number matters (3 unread, 4 open orders) and a dot when only “something new” does. Counts above 99 show 99+, and a count of 0 disappears — never show an empty badge.

### Colour

Red (error) for things that need attention, blue (discover) for new or info, and neutral for plain totals like the number of open orders on a tab. Small sits on 24px icons and tabs; medium next to larger content. On an icon it goes on the top-right corner, with a ring that separates it from the icon.

Wrap the icon: `<Badge count={3} label="unread notifications"><Icon … /></Badge>`. BottomNavbar items and Tabs items take a `badge` element. The number is visual; screen readers hear “3 unread notifications” from `label`.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `count` | `number` |  | Figma Type=Count (✏️ Count). Over max → “99+”; 0 hides it. |
| `dot` | `boolean` | `false` | Figma Type=Dot. |
| `color` | `'error' \| 'discover' \| 'neutral'` | `'error'` | Figma Color. |
| `size` | `'sm' \| 'md'` | `'sm'` | Figma Size: 16 (dot 8) · 20 (dot 12). |
| `label` | `string` |  | What it counts, for screen readers (“unread notifications”). Dot default: “New”. |
| `max / showZero` | `number / boolean` | `99 / false` | Cap before “+”; show a 0 count. |
| `children` | `ReactNode` |  | The icon it sits on — the badge goes on its top-right corner. |

## Tokens used

- `surface/accent/error-default · discover-default`
- `surface/inverted`
- `static/white`
- `content/inverted`
- `surface/primary (ring)`
- `size/08 · 12 · 16 · 20`
- `radius/full`
- `spacing/02 · 04`
- `Label/10 · 12`

## Recent changes

- **1.0.0** (2026-10-11) New: Badge: Badge: a count (3, 24, 99+) or a dot, in error, discover or neutral, small or medium, with a ring so it reads on top of an icon. A count of 0 hides it. Bottom navbar items and tabs can carry a badge (open orders on Portfolio, a count on an Orders tab). The number is visual only; screen readers hear it with what it counts, e.g. “3 unread notifications”. New Figma component L3: Badge (Type × Color × Size) on its own Badge page, with on-an-icon examples.
