# Color Palette

## Primary Colors

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Hot Pink** | `#FF1D6C` | `rgb(255, 29, 108)` | Primary accent, CTAs, active states |
| **Amber** | `#F5A623` | `rgb(245, 166, 35)` | Secondary accent, warnings |
| **Electric Blue** | `#2979FF` | `rgb(41, 121, 255)` | Links, info states |
| **Violet** | `#9C27B0` | `rgb(156, 39, 176)` | Tertiary accent, agent color |

## Neutral Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Black** | `#000000` | Primary background |
| **White** | `#FFFFFF` | Primary text |
| **Gray 900** | `#111111` | Card backgrounds |
| **Gray 800** | `#1A1A1A` | Elevated surfaces |
| **Gray 700** | `#333333` | Borders |
| **Gray 400** | `#666666` | Secondary text |
| **Gray 200** | `#999999` | Muted text |

## Semantic Colors

| Name | Hex | Usage |
|------|-----|-------|
| Success | `#22C55E` | Positive states, confirmations |
| Warning | `#F5A623` | Caution states (same as Amber) |
| Error | `#EF4444` | Error states, destructive actions |
| Info | `#2979FF` | Informational (same as Electric Blue) |

## Brand Gradient

The signature gradient uses golden ratio (φ) breakpoints at 38.2% and 61.8%:

```css
--gradient-brand: linear-gradient(
  135deg,
  #F5A623 0%,
  #FF1D6C 38.2%,
  #9C27B0 61.8%,
  #2979FF 100%
);
```

## Agent Colors

Each agent has a designated color from the brand palette:

| Agent | Color | Hex |
|-------|-------|-----|
| Octavia | Purple | `#9C27B0` |
| Lucidia | Cyan | `#00BCD4` |
| Alice | Green | `#22C55E` |
| Cipher | Blue | `#2979FF` |
| Prism | Yellow/Amber | `#F5A623` |
| Planner | White | `#FFFFFF` |

## Forbidden Colors

These colors are from the legacy system and must **never** be used:

| Hex | Reason |
|-----|--------|
| `#FF9D00` | Old amber — too orange |
| `#FF6B00` | Old accent — not in palette |
| `#FF0066` | Old pink — close but wrong |
| `#FF006B` | Old pink variant |
| `#D600AA` | Old magenta |
| `#7700FF` | Old purple — too saturated |
| `#0066FF` | Old blue — wrong shade |

If you encounter these in existing code, replace them with the correct values from the primary palette.

## Usage Guidelines

- Use **Hot Pink** sparingly — it's an accent, not a background
- Dark backgrounds should be pure `#000000`, not dark gray
- Text on dark backgrounds should be `#FFFFFF` at full opacity for primary, `#999999` for muted
- Never use more than two accent colors in the same component
- The brand gradient is for hero sections and key visual elements, not everyday components
