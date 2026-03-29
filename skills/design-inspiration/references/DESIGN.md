# Design System Strategy: The Curated Executive

## 1. Overview & Creative North Star
**The Creative North Star: "The Digital Concierge"**

This design system moves away from the cluttered, "database-style" aesthetic of traditional recruitment platforms. Instead, it adopts a high-end editorial approach—resembling a premium business journal or a luxury concierge service. The goal is to make every candidate feel like a "discovery" and every job post feel like an "opportunity."

We break the standard grid through **Intentional Asymmetry**. By utilizing generous whitespace (`spacing-24`) and overlapping editorial elements, we create a sense of depth and movement. This isn't just a platform; it's a curated experience for top-tier talent where the interface recedes to let the human profile shine.

---

## 2. Color & Tonal Soul
The palette is rooted in a deep, authoritative Purple, balanced by the warmth of a Soft Gold accent.

*   **Primary (`#5300b7`):** The "Executive Purple." Use this for high-impact brand moments and primary actions. Use the `primary_container` (`#6d28d9`) for larger interactive surfaces to maintain depth.
*   **Tertiary/Accent (`#5d3900` / `#ffb95f`):** The "Success Gold." Reserved strictly for premium status indicators, "Hired" states, or highlighting elite talent badges. It should feel like a gold-leaf stamp on a physical certificate.
*   **The "No-Line" Rule:** We do not use 1px solid borders to separate sections. Boundaries are defined by background shifts. A `surface_container_lowest` (#ffffff) card sits on a `surface` (#f7f9fb) background. The change in tone is the divider.
*   **Signature Textures:** For Hero sections, use a subtle linear gradient from `primary` to `primary_container` at a 135-degree angle. This adds a "silk-like" shimmer that flat hex codes cannot achieve.
*   **Glassmorphism:** Use `surface_container_low` with a 60% opacity and a `20px` backdrop-blur for floating navigation bars or modal overlays. This creates an integrated, modern "frosted glass" feel.

---

## 3. Typography: Editorial Authority
We utilize a dual-font strategy to balance global modernism with perfect Korean legibility.

*   **Display & Headline (Manrope):** Chosen for its geometric elegance. Use `display-lg` (3.5rem) for hero statements with tight letter-spacing (-0.02em) to create a bold, "magazine cover" impact.
*   **Title & Body (Pretendard/Inter):** Pretendard is our workhorse for Korean text, providing exceptional clarity at small sizes.
*   **The Hierarchy Rule:** Never use more than three levels of hierarchy on a single view. If everything is important, nothing is. Use `label-sm` in all-caps with `0.1rem` letter-spacing for category tags to create an "archival" look.

---

## 4. Elevation & Depth
We eschew traditional drop shadows in favor of **Tonal Layering**.

*   **The Layering Principle:**
    *   **Level 0 (Base):** `surface` (#f7f9fb)
    *   **Level 1 (Cards):** `surface_container_lowest` (#ffffff)
    *   **Level 2 (Popovers):** `surface_container_high` (#e6e8ea)
*   **Ambient Shadows:** If a floating element (like a FAB) requires a shadow, use a multi-layered blur: `0 10px 30px rgba(25, 28, 30, 0.04)`. The shadow must feel like a soft glow of light, not a dark smudge.
*   **The Ghost Border:** If accessibility requires a stroke, use `outline_variant` at 15% opacity. It should be felt, not seen.

---

## 5. Signature Components

### Buttons
*   **Primary:** High-gloss. Background `primary`, text `on_primary`. Shape `lg` (1rem). No border.
*   **Secondary:** The "Quiet Luxury" button. Background `transparent`, `outline` at 20% opacity. Text `primary`.
*   **Tertiary:** Text-only with a 2px underline that appears on hover, using the `tertiary_fixed` color.

### Elite Talent Cards
Forbid divider lines. Use `spacing-5` (1.7rem) of vertical padding to separate candidate details. Use a `surface_container_low` background for the header area of the card to subtly anchor the candidate's name.

### Input Fields
*   **State:** Default fields use `surface_container_lowest`. On focus, the background remains white, but the "Ghost Border" becomes `primary` at 100% opacity.
*   **Shape:** Always `md` (0.75rem).

### Status Chips
*   **Premium Status:** Use `tertiary_container` background with `on_tertiary_fixed` text. The contrast should feel like an "Award" rather than a "Notification."

---

## 6. Do's and Don'ts

### Do
*   **Do** use asymmetrical margins. If the left margin is `spacing-10`, try a `spacing-16` right margin for editorial layouts.
*   **Do** lean into the "Soft Gold" (`tertiary`) only for moments of celebration or extreme exclusivity.
*   **Do** use `display-md` for numerical data (like salary or years of experience) to make them visual anchors.

### Don't
*   **Don't** use a 1px #EEEEEE border for anything. Use a background color shift instead.
*   **Don't** use standard 400ms easing. Use a "Luxurious Ease" (e.g., `cubic-bezier(0.22, 1, 0.36, 1)`) for all transitions to make the UI feel responsive yet weighted.
*   **Don't** use pure black (#000000). Always use `on_surface` (#191c1e) for text to maintain a sophisticated softness.
