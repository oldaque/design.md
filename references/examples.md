# DESIGN.md Examples

Two complete, production-quality examples from the official
[google-labs-code/design.md](https://github.com/google-labs-code/design.md/tree/main/examples)
repository.

---

## Example 1 — Paws & Paths (warm, organic, Material-You palette)

A pet-services app with an energetic orange primary and calming blue secondary.
Uses a rich Material You palette with semantic color roles.

```markdown
---
name: Paws & Paths
colors:
  primary: "#855300"
  on-primary: "#ffffff"
  primary-container: "#f59e0b"
  on-primary-container: "#613b00"
  secondary: "#0058be"
  on-secondary: "#ffffff"
  secondary-container: "#2170e4"
  on-secondary-container: "#fefcff"
  tertiary: "#00658b"
  on-tertiary: "#ffffff"
  tertiary-container: "#1abdff"
  on-tertiary-container: "#004966"
  error: "#ba1a1a"
  on-error: "#ffffff"
  surface: "#f9f9ff"
  on-surface: "#151c27"
  surface-container-lowest: "#ffffff"
  surface-container-low: "#f0f3ff"
  surface-container: "#e7eefe"
  surface-container-high: "#e2e8f8"
  outline: "#867461"
  background: "#f9f9ff"
  on-background: "#151c27"
typography:
  display:
    fontFamily: Plus Jakarta Sans
    fontSize: 44px
    fontWeight: "800"
    lineHeight: 52px
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Plus Jakarta Sans
    fontSize: 32px
    fontWeight: "700"
    lineHeight: 40px
    letterSpacing: -0.01em
  headline-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 24px
    fontWeight: "700"
    lineHeight: 32px
  body-lg:
    fontFamily: Plus Jakarta Sans
    fontSize: 18px
    fontWeight: "400"
    lineHeight: 28px
  body-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 16px
    fontWeight: "400"
    lineHeight: 24px
  label-md:
    fontFamily: Plus Jakarta Sans
    fontSize: 14px
    fontWeight: "600"
    lineHeight: 20px
    letterSpacing: 0.01em
  label-sm:
    fontFamily: Plus Jakarta Sans
    fontSize: 12px
    fontWeight: "500"
    lineHeight: 16px
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  base: 8px
  xs: 4px
  sm: 12px
  md: 24px
  lg: 40px
  xl: 64px
  gutter: 16px
  margin: 24px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.lg}"
    padding: "{spacing.md}"
  button-primary-hover:
    backgroundColor: "{colors.primary-container}"
    textColor: "{colors.on-primary-container}"
  button-secondary:
    backgroundColor: "{colors.secondary}"
    textColor: "{colors.on-secondary}"
    typography: "{typography.label-md}"
    rounded: "{rounded.lg}"
    padding: "{spacing.md}"
  button-secondary-hover:
    backgroundColor: "{colors.secondary-container}"
    textColor: "{colors.on-secondary-container}"
  card-profile:
    backgroundColor: "{colors.surface-container-lowest}"
    rounded: "{rounded.xl}"
    padding: "{spacing.md}"
  card-walk-stat:
    backgroundColor: "{colors.secondary-container}"
    textColor: "{colors.on-secondary-container}"
    rounded: "{rounded.md}"
    padding: "{spacing.sm}"
  input-field:
    backgroundColor: "{colors.surface-container-low}"
    textColor: "{colors.on-surface}"
    typography: "{typography.body-md}"
    rounded: "{rounded.DEFAULT}"
    padding: "{spacing.sm}"
  badge-status:
    backgroundColor: "{colors.tertiary-container}"
    textColor: "{colors.on-tertiary-container}"
    typography: "{typography.label-sm}"
    rounded: "{rounded.full}"
    padding: "{spacing.xs}"
---

## Brand & Style

The design system is built to evoke the joyful energy of a walk in the park
balanced with the reliability of a premium professional service. The brand
personality is optimistic, trustworthy, and active.

The chosen style is **Modern Corporate** with a friendly, human-centric twist.
It utilizes clean layouts and significant whitespace to reduce cognitive load
for busy pet owners. The interface feels light and airy, avoiding heavy borders
in favor of soft shadows and tonal shifts.

## Colors

The palette centers on "Golden Retriever" orange to drive action and signal
energy. This is balanced by "Sky Walk" blue, which provides a calming
counterpoint for scheduling and administrative tasks.

- **Primary (#855300):** Main actions, active states, and highlights.
- **Secondary (#0058be):** Trust indicators and navigation accents.
- **Neutral:** Soft grays for backgrounds and borders.

## Typography

**Plus Jakarta Sans** — friendly, rounded terminals and exceptional legibility.
Headlines use bold weights for clear hierarchy; body uses generous line heights
for a premium, clean feel.
```

---

## Example 2 — Atmospheric Glass (dark, ethereal, frosted-glass aesthetic)

A dark-mode app with a deep navy base and translucent glass-effect components.

```markdown
---
name: Atmospheric Glass
colors:
  primary: "#ffffff"
  on-primary: "#2f3131"
  primary-container: "#e2e2e2"
  secondary: "#adc9eb"
  on-secondary: "#14324e"
  secondary-container: "#304b68"
  on-secondary-container: "#9fbbdd"
  error: "#ffb4ab"
  on-error: "#690005"
  surface: "#0b1326"
  on-surface: "#dae2fd"
  surface-container-low: "#131b2e"
  surface-container: "#171f33"
  surface-container-high: "#222a3d"
  outline: "#8e9192"
  background: "#0b1326"
  on-background: "#dae2fd"
typography:
  display-lg:
    fontFamily: Inter
    fontSize: 84px
    fontWeight: "700"
    lineHeight: 90px
    letterSpacing: -0.04em
  headline-lg:
    fontFamily: Inter
    fontSize: 32px
    fontWeight: "600"
    lineHeight: 40px
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Inter
    fontSize: 24px
    fontWeight: "500"
    lineHeight: 32px
  body-lg:
    fontFamily: Inter
    fontSize: 18px
    fontWeight: "400"
    lineHeight: 28px
  body-md:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: "400"
    lineHeight: 24px
  label-sm:
    fontFamily: Inter
    fontSize: 12px
    fontWeight: "600"
    lineHeight: 16px
    letterSpacing: 0.05em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  unit: 8px
  container-padding: 24px
  card-gap: 16px
  section-margin: 40px
  glass-padding: 20px
components:
  glass-card-standard:
    backgroundColor: rgba(255, 255, 255, 0.1)
    textColor: "{colors.primary}"
    rounded: "{rounded.lg}"
    padding: "{spacing.glass-padding}"
  glass-card-elevated:
    backgroundColor: rgba(255, 255, 255, 0.2)
    textColor: "{colors.primary}"
    rounded: "{rounded.xl}"
    padding: "{spacing.glass-padding}"
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-sm}"
    rounded: "{rounded.xl}"
    height: 48px
    padding: 0 24px
  button-primary-hover:
    backgroundColor: "{colors.primary-container}"
  button-ghost:
    backgroundColor: rgba(255, 255, 255, 0.05)
    textColor: "{colors.primary}"
    rounded: "{rounded.xl}"
    padding: 0 24px
    height: 48px
  input-field:
    backgroundColor: rgba(255, 255, 255, 0.08)
    textColor: "{colors.on-surface}"
    rounded: "{rounded.lg}"
    padding: "{spacing.glass-padding}"
---

## Overview

A cinematic dark interface evoking the sensation of looking through frosted
glass at a deep night sky. The aesthetic is ethereal, premium, and slightly
mysterious — suited for creative, media, or entertainment applications.

## Colors

Deep navy base with white as the primary interactive color. Blue-grey
secondary for informational elements.

- **Primary (#ffffff):** All interactive actions against the dark surface.
- **Secondary (#adc9eb):** Trust indicators and informational tones.
- **Surface (#0b1326):** Deep navy for all page backgrounds.

## Typography

**Inter** at all scales — precise, geometric, legible against dark backgrounds.
Display sizes use extreme weights and negative tracking for drama.

## Elevation & Depth

Depth is achieved through **translucent glass layers** (rgba backgrounds) rather
than traditional shadows. Higher-elevation components use greater opacity.
Standard glass: 10% white opacity. Elevated glass: 20%.

## Do's and Don'ts

- Do use translucent rgba values for glass components, not solid colors.
- Don't add drop shadows — they break the glass aesthetic.
- Do keep text on glass at maximum contrast (white on dark glass).
```
