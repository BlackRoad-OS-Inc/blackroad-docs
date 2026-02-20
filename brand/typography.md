# Typography

## Font Stack

```css
font-family: -apple-system, BlinkMacSystemFont, 'SF Pro Display', 'Segoe UI', Roboto, sans-serif;
```

For code and monospace contexts:

```css
font-family: 'SF Mono', 'Fira Code', 'JetBrains Mono', 'Cascadia Code', monospace;
```

## Line Height

All line heights follow the golden ratio:

```css
line-height: 1.618;
```

This applies to body text, headings, and all readable content. Exceptions are allowed for tightly packed UI elements like badges and labels where `1.2` is acceptable.

## Type Scale

The scale is based on a 16px base with a 1.25 ratio (major third):

| Level | Size | Weight | Usage |
|-------|------|--------|-------|
| `xs` | 12px / 0.75rem | 400 | Captions, labels |
| `sm` | 14px / 0.875rem | 400 | Secondary text, metadata |
| `base` | 16px / 1rem | 400 | Body text |
| `lg` | 20px / 1.25rem | 500 | Subheadings |
| `xl` | 25px / 1.563rem | 600 | Section headings |
| `2xl` | 31px / 1.953rem | 700 | Page headings |
| `3xl` | 39px / 2.441rem | 700 | Hero headings |
| `4xl` | 49px / 3.052rem | 800 | Display text |

## Font Weights

| Weight | Value | Usage |
|--------|-------|-------|
| Regular | 400 | Body text, descriptions |
| Medium | 500 | Subheadings, emphasis |
| Semibold | 600 | Section headings, labels |
| Bold | 700 | Page headings, strong emphasis |
| Extrabold | 800 | Display text, hero elements |

## Color Application

- Primary text on dark: `#FFFFFF`
- Secondary text on dark: `#999999`
- Muted text on dark: `#666666`
- Accent text: Use brand colors (Hot Pink for links/actions, Amber for highlights)

## Letter Spacing

- Headings (`xl` and above): `-0.02em` (slightly tighter)
- Body text: `0` (default)
- All caps labels: `0.05em` (slightly wider)

## Guidelines

- Never use italic for emphasis in headings — use color or weight instead
- Avoid all-caps for body text; reserve for short labels and badges
- Minimum text size for accessibility: 14px
- Ensure sufficient contrast ratio: 4.5:1 for body text, 3:1 for large text
