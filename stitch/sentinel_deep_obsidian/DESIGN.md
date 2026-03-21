# Design System Strategy: The Sovereign Interface

## 1. Overview & Creative North Star

### The Creative North Star: "The Digital Curator"
In an industry saturated with "hacker-green" terminal aesthetics and aggressive neon glows, this design system takes an opposing, high-end editorial stance. We treat security data not as a chaotic stream to be tamed, but as a curated gallery of insights. "The Digital Curator" philosophy emphasizes **intentional asymmetry**, **monolithic depth**, and **tonal authority**.

The system moves beyond the standard SaaS grid by utilizing expansive negative space and a sophisticated "layer-on-layer" approach. We eschew traditional structural lines in favor of tonal shifts, creating an interface that feels carved from a single block of obsidian rather than assembled from a kit of parts.

---

## 2. Colors & Surface Philosophy

### Tonal Foundation
The palette is rooted in a deep, atmospheric neutral base, allowing the Indigo and Blue accents to function as precise instruments of attention rather than overwhelming decorations.

*   **Background:** `#111317` (Deep off-black)
*   **Primary (Accent):** `#c0c1ff` (Indigo-tinted light)
*   **On-Primary Container:** `#0d0096` (Deep Contrast)
*   **Surface Tiers:**
    *   `surface_container_lowest`: `#0c0e12` (Recessed areas)
    *   `surface_container_low`: `#1a1c20` (Standard sections)
    *   `surface_container_highest`: `#333539` (Actionable cards)

### The "No-Line" Rule
**Explicit Instruction:** Designers are prohibited from using 1px solid borders to define section boundaries. 
Structure must be achieved through:
1.  **Background Shifts:** Place a `surface_container_highest` card atop a `surface_container_low` background.
2.  **Negative Space:** Use the **8 (2rem)** or **12 (3rem)** spacing tokens to separate logical groups.

### The "Glass & Gradient" Rule
To elevate the UI into the "premium" tier, floating elements (modals, dropdowns, or hovering tooltips) should utilize a **Glassmorphism** effect:
*   **Fill:** `surface_variant` at 60% opacity.
*   **Backdrop Blur:** 12px to 20px.
*   **Signature Texture:** Primary CTAs should not be flat. Apply a subtle linear gradient from `primary` (#c0c1ff) to `primary_container` (#8083ff) at a 135-degree angle to provide a "metallic" finish.

---

## 3. Typography

The system utilizes a dual-font pairing to balance authority with utility.

*   **Display & Headline (Manrope):** Used for high-level data points and section titles. Manrope’s geometric yet warm proportions convey modern sophistication. 
    *   *Usage:* `headline-lg` (2rem) for dashboard overview titles to establish a strong editorial anchor.
*   **Title, Body & Labels (Inter):** The workhorse for high-density security data. 
    *   *Usage:* `body-sm` (0.75rem) for technical logs and `label-md` for status indicators.

**Hierarchy Note:** Always maintain a high contrast between `on_surface` (for primary content) and `on_surface_variant` (for metadata). This ensures the eye naturally skips over secondary information until it is needed.

---

## 4. Elevation & Depth

### The Layering Principle
Depth is achieved through "Tonal Stacking."
*   **Level 0:** `background` (#111317) - The canvas.
*   **Level 1:** `surface_container_low` (#1a1c20) - The primary workspace regions.
*   **Level 2:** `surface_container_highest` (#333539) - Interactive cards and data modules.

### Ambient Shadows
Avoid the "drop shadow" look. Use **Ambient Glows**:
*   **Shadow Color:** A tinted version of `surface_tint` at 6% opacity.
*   **Blur/Spread:** 40px blur, 0px spread. This mimics a soft, natural lift caused by backlighting rather than a harsh top-down light source.

### The "Ghost Border" Fallback
If a border is required for accessibility (e.g., in high-contrast mode), use a "Ghost Border":
*   **Token:** `outline_variant` (#464554) at **15% opacity**.
*   **Stroke:** 1px inside. Never use 100% opaque borders.

---

## 5. Components

### Buttons & Interaction
*   **Primary:** Indigo gradient (`primary` to `primary_container`) with `on_primary_fixed` text. **Roundedness:** `md` (0.375rem).
*   **Secondary:** Ghost style. No background, `outline_variant` at 20% opacity.
*   **States:** On hover, primary buttons should increase in `surface_bright` luminescence rather than changing color entirely.

### Cards & Data Modules
*   **Constraint:** Forbid divider lines within cards.
*   **Separation:** Use `surface_container_lowest` (#0c0e12) as a subtle "well" for nested data lists within a card to create internal hierarchy.

### Status Chips (Security Context)
*   **Neutral/Info:** Use `secondary_container` with `on_secondary_container` text.
*   **Critical/Alert:** Use `error_container` with `on_error_container` text.
*   **Styling:** Pill-shaped (`rounded-full`), small-caps `label-sm` typography.

### Data Inputs
*   **Field Style:** Flat `surface_container_highest` fill. 
*   **Focus State:** Instead of a thick border, use a subtle 1px "Ghost Border" at 40% opacity and a 2px outer glow of the `primary` color at 10% opacity.

---

## 6. Do’s and Don’ts

### Do:
*   **Use Asymmetry:** Place large headline text on the left with significant white space to the right to create an "Editorial" layout.
*   **Embrace "Invisible" Containers:** Trust the tonal shifts between `surface_container` tokens to define the layout.
*   **Tint Your Shadows:** Always use a faint indigo/blue tint in your shadows to maintain the "cool" atmospheric feel.

### Don't:
*   **Don't use Green:** While traditional for "Safe" states, use Indigo or White for "Model Ready" / "Healthy" states to avoid the "hacker" cliché.
*   **Don't use 100% Black:** Never use #000000. It kills the depth of the "Digital Curator" aesthetic. Stick to the `background` token (#111317).
*   **Don't use Flashy Motion:** Transitions should be "weighted" (e.g., 300ms Ease-Out) to feel like physical machinery rather than a bouncy consumer app.