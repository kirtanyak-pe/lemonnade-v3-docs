# Empty state

> What to show when a search or list has nothing in it: an illustration, a short heading and hint, and a way forward.

- Group: Feedback & status
- Lifecycle: done
- Status: Figma synced
- Version: 1.3.0
- Figma: https://www.figma.com/design/lxQ6QIXGOv5mmx0khh5sJn/?node-id=4543-66488
- Source: `src/components/EmptyState`
- Also called: No results, zero state, blank slate, nothing found

## Import

```tsx
import { EmptyState, EmptyStateIllustration, NoResultsIllustration } from './components/EmptyState' // path relative to src/
```

## Overview

### Say what happened, then offer a way out

Use the heading to say what's missing ("No results found") and the description to say what to try. Figma's CTA is a small primary Button, "Clear" with a delete icon, that resets the search. Tap it in the demo above, then type to see results come back.

### Illustration

Every empty state uses the same illustration: Figma's **L3: Empty state default illustration** (a document with a ✕), which is already in the Illustration slot. Keep it — whenever an empty state needs an illustration, it's this one. It follows the theme, including the brand color. Hide it only when space is tight.

The default is `EmptyStateIllustration` (Figma 5599:7131) — use it for every empty state; don't pass other artwork. It's an inline SVG and every fill is a token, so it follows the theme, including the brand color. Pass `null` to `illustration` to hide it. `NoResultsIllustration` (the old magnifier, Figma "L3: No search illustration") is still exported for screens that already use it.

### Layout and accessibility

The empty state fills the space it's in and centres itself; Figma's frame is a fixed 412px tall. If results change as someone types, the result count should be announced to screen readers — note it in the handoff.

The empty state fills its flex parent and centres itself; Figma's frame is a fixed 412px tall. The heading is an h2 by default (use `headingLevel={3}` under a section heading). The illustration is hidden from screen readers. If results change as someone types, announce the count separately, for example in a live region next to the search field.

## Do / Don't

### Offer a way forward

- ✅ **Do:** Say what happened and give an action to recover.
- ❌ **Don't:** Leave a blank screen or a dead end.

## Options (tree)

Empty state — Nothing to show, or no results

- **Parts**
  - `illustration + title + description + action` — Say what happened and give a way to recover.
  - `title only` — Small spaces; still never a dead end.
- **Heading level**
  - `h2` — Default: the empty state is the page content.
  - `h3` — Inside a section that already has an h2.

## Props

| Prop | Type | Default | Description |
|---|---|---|---|
| `title` | `ReactNode` |  | Figma ✏️ Heading. |
| `description` | `ReactNode` |  | Figma ✏️ Description. |
| `illustration` | `ReactNode \| null` | `<EmptyStateIllustration />` | Figma Illustration slot, 120px — the L3 default illustration. Keep the default; null hides it. |
| `action` | `ReactNode` |  | Figma Clear CTA: usually <Button size="sm" variant="primary">. |
| `headingLevel` | `2 \| 3` | `2` | Heading element for the title. |

## Tokens used

- `surface/default`
- `content/primary · secondary`
- `surface/accent/brand-default · brand-light (illustration)`
- `surface/inverted · quaternary (illustration)`
- `static/black (illustration)`
- `Heading/16`
- `Label/14`
- `spacing/04 · 16 · 24`
- `size/illustration (local, 120px)`

## Recent changes

- **1.3.0** (2026-10-16) New default illustration: The illustration is now L3: Empty state default illustration (a document with a ✕), the same artwork as Figma's Illustration slot. Every empty state uses it. Its colours follow the theme (brand, brand-light, inverted, quaternary). EmptyStateIllustration (inline SVG, token fills) is the illustration default. NoResultsIllustration (the old magnifier) is still exported.
- **1.2.0** (2026-10-05) Typography role rules: Typography role rules: the line under the title uses Description (it describes the heading). Matches Figma.
- **1.1.0** (2026-10-05) New type weights: Text uses the three typography roles: titles Heading (750), labels Label (650).
