---
name: Design Inspiration
description: This skill should be used when the user asks to "create a UI component", "design a page", "update the design", "change styles", "build a new screen", or when any frontend design work involves creating or modifying CSS, HTML templates, or React components. It provides design reference images that Claude should analyze and match when generating new designs.
---

# Design Inspiration

This skill ensures all frontend design work maintains visual consistency with the user's preferred design style by referencing the Design System document (`references/DESIGN.md`) and uploaded inspiration images before generating or modifying any UI code.

## When This Skill Activates

- Creating new pages, components, or screens
- Modifying existing CSS, styles, or visual layouts
- Building UI elements (cards, buttons, headers, forms, etc.)
- Refactoring or redesigning frontend interfaces
- Any task that produces visible UI output

## Workflow

### Step 0: Load Design System Document

Before any design work, read the Design System document:

```
.claude/skills/design-inspiration/references/DESIGN.md
```

This is the **authoritative design specification** — "The Curated Executive" strategy. It defines:

- **Color & Tonal Soul**: Executive Purple (`#5300b7`) + Success Gold (`#ffb95f`) palette
- **The "No-Line" Rule**: No 1px borders — use background color shifts instead
- **Typography**: Manrope for display/headlines, Pretendard/Inter for body
- **Tonal Layering**: Surface elevation via background tones, not drop shadows
- **Signature Components**: Buttons, cards, inputs, status chips specifications
- **Do's and Don'ts**: Strict rules (no pure black, luxurious easing, asymmetric margins)

All design output MUST comply with DESIGN.md rules. When DESIGN.md conflicts with `globals.css`, DESIGN.md takes precedence for new work.

### Step 1: Load Reference Images

Before writing any design-related code, read all image files in the `images/` directory relative to this skill:

```
.claude/skills/design-inspiration/images/
```

Use the Read tool to view each image file (PNG, JPG, WebP, SVG). These images represent the user's desired design direction.

If the `images/` folder is empty, notify the user:
> "Design Inspiration 폴더(`/.claude/skills/design-inspiration/images/`)에 레퍼런스 이미지가 없습니다. 원하는 디자인 스타일의 이미지를 추가해주세요."

Then proceed without image references, falling back to the project's existing style tokens in `globals.css`.

### Step 2: Analyze Design Patterns

Combine insights from DESIGN.md and reference images to build a unified design direction:

**From DESIGN.md (always apply):**
- Executive Purple + Soft Gold color system
- No-Line Rule (background shifts, not borders)
- Tonal Layering for elevation (surface → surface_container_lowest → surface_container_high)
- Luxurious Ease transitions (`cubic-bezier(0.22, 1, 0.36, 1)`)
- Manrope headlines, Pretendard body, max 3 hierarchy levels per view

**From reference images (extract and layer on top):**
- Color accent variations or additional palette notes
- Spacing & Layout rhythm, grid structure, whitespace usage
- Component shapes, specific border-radius preferences
- Visual effects: gradients, overlays, blur, glassmorphism usage
- Overall tone refinements

### Step 3: Apply to Output

When generating or modifying frontend code:

1. **Match the visual tone** of the reference images — not a pixel copy, but the same design language
2. **Reuse existing project tokens** from `globals.css` where they align with the references
3. **Propose new tokens** only when the references demand a style not covered by existing variables
4. **Maintain consistency** across all generated components in the same session

### Step 4: Explain Design Decisions

After generating code, briefly note which reference image(s) influenced key decisions:
- "레퍼런스 이미지의 둥근 카드 스타일을 반영하여 `border-radius: 24px` 적용"
- "레퍼런스의 색상 팔레트에서 추출한 그라디언트 사용"

## Priority Order (Conflict Resolution)

When sources conflict, apply in this priority:

1. **`references/DESIGN.md`** — The authoritative design system (highest priority)
2. **`images/` reference images** — Visual direction supplements
3. **`globals.css`** — Existing project tokens (align or propose updates)
4. **CLAUDE.md Style Guide** — General project conventions

For **new components**: follow DESIGN.md strictly, supplemented by images.
For **existing component modifications**: maintain current tokens unless user explicitly requests a redesign toward DESIGN.md.

## Managing Reference Images

The `images/` folder accepts:
- **Screenshots** of websites or apps with desired aesthetics
- **Dribbble/Behance exports** of UI inspiration
- **Mood board images** showing color/typography preferences
- **Existing design mockups** (Figma exports, etc.)

Naming convention (recommended):
- `01-overall-style.png` — General mood/direction
- `02-card-design.png` — Specific component reference
- `03-color-palette.png` — Color scheme reference
- `dashboard-ref.png` — Page-specific reference

## Additional Resources

### Reference Files

- **`references/DESIGN.md`** — The Curated Executive design system (color, typography, elevation, components, do's/don'ts)
- **`references/analysis-guide.md`** — How to extract design patterns from reference images systematically
