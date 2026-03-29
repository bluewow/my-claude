# Design Analysis Guide

Systematic approach to extracting actionable design patterns from reference images.

## Color Extraction

When analyzing reference images for color:

1. **Primary color**: The most dominant non-neutral color
2. **Secondary color**: Supporting color used for accents or sections
3. **Neutral palette**: Background, surface, and text colors
4. **Semantic colors**: Success (green), warning (amber), error (red), info (blue)
5. **Gradient patterns**: Direction, color stops, opacity transitions

Map extracted colors to existing CSS variables in `globals.css` where possible. Create new variables only for genuinely new colors.

## Typography Patterns

Extract from reference images:

| Property | What to look for |
|----------|-----------------|
| Headings | Weight (bold/black), size ratio to body, letter-spacing |
| Body text | Line-height, font-size, paragraph spacing |
| Labels | Uppercase usage, tracking (letter-spacing), font-weight |
| Hierarchy | How many distinct text levels visible |

## Spacing System

Identify the spacing rhythm:

- **Base unit**: Most common smallest gap (4px, 8px, etc.)
- **Scale**: How spacing grows (linear: 8/16/24, or geometric: 4/8/16/32)
- **Padding patterns**: Card inner padding, section padding
- **Gap patterns**: Space between cards, list items, form fields

## Component Patterns

### Cards
- Corner radius (sharp, slightly rounded, very rounded, pill)
- Shadow depth (flat, subtle, elevated, dramatic)
- Border usage (none, hairline, prominent)
- Background (solid, gradient, glass/blur)

### Buttons
- Shape (rectangle, rounded, pill)
- Fill style (solid, outline, ghost, gradient)
- Size variants visible
- Hover/active state clues

### Navigation
- Position (top, side, bottom)
- Style (tabs, links, icons, pills)
- Active state indicator

### Forms
- Input style (underline, bordered, filled)
- Label position (above, floating, inline)
- Validation display pattern

## Mood & Atmosphere

Classify the overall design feeling:

| Dimension | Spectrum |
|-----------|----------|
| Weight | Light ←→ Heavy |
| Temperature | Cool ←→ Warm |
| Formality | Casual ←→ Formal |
| Density | Spacious ←→ Dense |
| Motion | Static ←→ Dynamic |
| Complexity | Minimal ←→ Rich |

## Applying Analysis to Code

After analysis, translate findings into concrete CSS:

```css
/* Example: Extracted from reference showing soft, modern card UI */
--ref-radius: 24px;        /* Very rounded corners observed */
--ref-shadow: 0 8px 32px rgba(0,0,0,0.08);  /* Soft elevated shadow */
--ref-padding: 24px;       /* Generous inner padding */
--ref-gap: 16px;           /* Consistent spacing between elements */
```

Compare with existing project tokens before adding new ones. Prefer extending the existing system over creating parallel variables.
