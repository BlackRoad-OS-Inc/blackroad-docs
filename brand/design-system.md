# BlackRoad Design System

The BlackRoad design system is built on mathematical harmony — the golden ratio (φ = 1.618) governs spacing, typography, and color distribution.

## Foundations

| Foundation | Reference |
|-----------|-----------|
| [Colors](colors.md) | Primary palette, gradients, forbidden colors |
| [Typography](typography.md) | Font families, scale, line heights |
| [Spacing](spacing.md) | Golden ratio spacing system |

## Core Principles

1. **Dark-first** — All interfaces default to dark backgrounds (#000000). Light mode is secondary.
2. **Golden ratio** — Spacing, line heights, and gradient stops follow φ = 1.618.
3. **Hot Pink is primary** — #FF1D6C is the primary accent. Use it for CTAs, active states, and key highlights.
4. **Restraint** — Use accent colors sparingly. Most of the UI is black/white with strategic color accents.
5. **No forbidden colors** — Never use the legacy color palette. See [colors](colors.md) for the forbidden list.

## Brand Gradient

The signature gradient uses golden ratio stops:

```css
background: linear-gradient(
  135deg,
  #F5A623 0%,       /* Amber */
  #FF1D6C 38.2%,    /* Hot Pink — golden ratio */
  #9C27B0 61.8%,    /* Violet — golden ratio */
  #2979FF 100%      /* Electric Blue */
);
```

## Component Patterns

### Cards

- Background: `rgba(255, 255, 255, 0.05)` on dark
- Border: `1px solid rgba(255, 255, 255, 0.1)`
- Border radius: `13px` (golden ratio spacing)
- Padding: `21px` (golden ratio spacing)

### Buttons

- Primary: Hot Pink background, white text
- Secondary: Transparent with white border
- Ghost: No background, Hot Pink text on hover

### Status Indicators

- Active/Success: `#22C55E` (green)
- Warning: `#F5A623` (amber)
- Error: `#EF4444` (red)
- Info: `#2979FF` (electric blue)

## CSS Custom Properties

All brand values are available as CSS custom properties. See the `app/globals.css` file in `blackroad-web` for the complete set.

## Related Documents

- [Colors](colors.md)
- [Typography](typography.md)
- [Spacing](spacing.md)
