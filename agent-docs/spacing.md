# Spacing & radius

> Spacing, radius, size and icon-size tokens from the Figma "🌌 Number" and "Icon size" collections.

- Group: Foundations
- Lifecycle: done
- Version: 1.1.0

## Overview

### Tile corners (test)

Figma's **🌌 Number** collection has a second mode, **Tile (sharp corners)**, where every radius (radius-00 to radius-32 and radius-full) is 0, for a square look like Windows tiles. **Mode 1** keeps today's corners and stays the default.

To try it here, open the brand menu in the header and turn on **Tile corners (test)**. In code every component turns square, round ones too (radio buttons, switches, avatars, dots). Figma differs for two of them: radio buttons are drawn as circles and the switch track as a fixed shape, so they stay round there (avatars do turn square). Whether those should stay round is still to decide. It's a test, not a new rule; the choice is remembered only in this browser.

`data-shape="tile"` on `<html>` or any wrapper sets every `--l3-radius-*` to `0px` (`src/tokens/generated/shape.css`, built by `npm run tokens` from `src/tokens/source/local.shape.json`). `useTheme().shape` is `'rounded' | 'tile'`; `setShape()` saves it with the theme (`l3-theme`), and the pre-paint script in `index.html` applies it. Portaled parts (`BottomSheet`) follow a wrapper only when their `container` is inside it; otherwise set it on `<html>`.

## Recent changes

- **1.1.0** (2026-10-15) Tile corners (test): New mode in Figma "🌌 Number": Tile (sharp corners), where every radius (radius-00 … radius-32 and radius-full) is 0. Mode 1 keeps today’s corners. Tile corners (test): turn it on in the header brand menu to see every component with square corners. A test only; the default stays rounded. data-shape="tile" on <html> or any wrapper sets every --l3-radius-* to 0px (generated/shape.css from source/local.shape.json via npm run tokens). useTheme().shape ('rounded' | 'tile') + setShape(), saved in l3-theme and applied before first paint.
- **1.0.0** (2026-09-25) Spacing, radius and size from Figma: Spacing, radius, size and icon-size variables from the Figma "🌌 Number" and "Icon size" collections.
