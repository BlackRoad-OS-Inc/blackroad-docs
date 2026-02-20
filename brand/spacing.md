# Spacing

All spacing in BlackRoad OS follows the golden ratio (φ = 1.618). Each step is the previous step multiplied by φ, rounded to the nearest pixel.

## Spacing Scale

| Token | Value | Calculation | Common Usage |
|-------|-------|-------------|--------------|
| `--space-2xs` | 5px | 8 / φ | Icon gaps, tight inline spacing |
| `--space-xs` | 8px | Base | Small gaps, inline spacing |
| `--space-sm` | 13px | 8 × φ | Component internal padding |
| `--space-md` | 21px | 13 × φ | Card padding, section gaps |
| `--space-lg` | 34px | 21 × φ | Section spacing, large padding |
| `--space-xl` | 55px | 34 × φ | Page section margins |
| `--space-2xl` | 89px | 55 × φ | Major section breaks |
| `--space-3xl` | 144px | 89 × φ | Hero spacing, page margins |

## CSS Custom Properties

```css
:root {
  --space-2xs: 5px;
  --space-xs: 8px;
  --space-sm: 13px;
  --space-md: 21px;
  --space-lg: 34px;
  --space-xl: 55px;
  --space-2xl: 89px;
  --space-3xl: 144px;
}
```

## Usage Guidelines

### Component Internal Spacing

- Badge padding: `xs` (8px) horizontal, `2xs` (5px) vertical
- Button padding: `sm` (13px) horizontal, `xs` (8px) vertical
- Card padding: `md` (21px)
- Input padding: `sm` (13px) horizontal, `xs` (8px) vertical

### Layout Spacing

- Gap between grid items: `md` (21px)
- Gap between sections: `lg` (34px) or `xl` (55px)
- Page top/bottom margin: `xl` (55px) or `2xl` (89px)
- Sidebar width: `233px` (144 × φ, rounded)

### Responsive Adjustments

On smaller screens, drop one level in the scale:

| Desktop | Mobile |
|---------|--------|
| `xl` (55px) | `lg` (34px) |
| `lg` (34px) | `md` (21px) |
| `md` (21px) | `sm` (13px) |

## Border Radius

Border radius also follows the golden ratio scale:

| Size | Value | Usage |
|------|-------|-------|
| `sm` | 5px | Badges, small elements |
| `md` | 8px | Buttons, inputs |
| `lg` | 13px | Cards, containers |
| `xl` | 21px | Modals, large containers |
| `full` | 9999px | Circular elements, pills |

## Why Golden Ratio?

The golden ratio appears throughout nature and art as a proportion that humans find naturally harmonious. By basing our spacing system on φ, every element in the UI relates to every other element through the same underlying proportion. This creates visual coherence without requiring manual adjustment.
