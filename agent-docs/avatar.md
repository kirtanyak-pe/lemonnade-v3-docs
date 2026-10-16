# Avatar

> An avatar shows a stock or company logo, a fund, or a person — a picture, initials or an icon — at the start of a row, card or header.

- Group: Data display
- Lifecycle: done
- Status: Figma synced
- Version: 1.0.1
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=5480-23074
- Source: `src/components/Avatar`
- Also called: Stock logo, company logo, profile picture, initials, user icon, thumbnail

## Import

```tsx
import { Avatar } from './components/Avatar' // path relative to src/
```

## Overview

### Picture, then initials, then icon

Use the company's logo when you have it. Without one, show its initials (1–2 letters, e.g. RI for Reliance) — and if a logo fails to load, initials take its place automatically. The person icon is for people with no photo and for generic actions like “Add a nominee”.

### Shape and size

Circle for people and stocks; rounded for apps, mutual funds and documents. Use 40 in list rows, 32 or 24 in dense rows and chips, and 48 in headers. Every avatar has a grey fill and a light edge, so white logos never disappear on a white screen.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `src` | `string` |  | Figma Type=Image: logo or photo. Falls back to initials, then the icon, if it fails to load. |
| `initials` | `string` |  | Figma Type=Initials (✏️ Initials): 1–2 letters. |
| `icon` | `ReactNode` |  | Figma Type=Icon (↪ Icon). Defaults to a person. |
| `size` | `24 \| 32 \| 40 \| 48` | `40` | Figma Size. |
| `shape` | `'circle' \| 'rounded'` | `'circle'` | Figma Shape. |
| `label` | `string` |  | Screen-reader name. Leave out when the name is written next to it (rows, cards). |

## Tokens used

- `surface/secondary · tertiary`
- `border/light`
- `content/primary · secondary`
- `size/24 · 32 · 40 · 48`
- `radius/full · 06 · 08 · 12`
- `Label/10 · 12 · 14 · 16`
- `icon-size/14 · 18 · 20 · 24`

## Recent changes

- **1.0.1** (2026-10-11) Figma sync: icon colour, logo edge: The person icon is content-secondary at 60%. A logo or photo sits 1px inside the avatar's border with its own light edge, so white logos keep a clear outline.
- **1.0.0** (2026-10-11) New: Avatar: Avatar for stock and company logos, funds and people: a picture, 1–2 initials or an icon, in 24 · 32 · 40 · 48, circle or rounded. A logo that fails to load falls back to initials. New Figma component L3: Avatar (Type × Size × Shape) on its own Avatar page. List cell: an avatar in the left slot keeps its own size (e.g. 40 for holdings rows) instead of shrinking to the 24px icon box.
