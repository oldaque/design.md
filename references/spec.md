# DESIGN.md Specification Reference

> Source: [google-labs-code/design.md](https://github.com/google-labs-code/design.md) · version: **alpha**

---

## File Structure

A DESIGN.md file has two layers:

```
---
<YAML frontmatter: design tokens>
---

## Section Heading
<Markdown prose: design rationale>
```

- The YAML block must be delimited by lines containing exactly `---`.
- The Markdown body uses `##` headings for all sections.
- An optional `#` heading may appear for document titling — it is not parsed as a section.

---

## Token Schema (YAML Frontmatter)

```yaml
version: <string>          # optional — current: "alpha"
name: <string>             # required — project name
description: <string>      # optional — one-line summary

colors:
  <token-name>: <Color>

typography:
  <token-name>: <Typography>

rounded:
  <scale-level>: <Dimension>

spacing:
  <scale-level>: <Dimension | number>

components:
  <component-name>:
    <property>: <value | token-reference>
```

---

## Token Types

### Color
A hex string in the sRGB color space.

```yaml
primary: "#1A1C1E"
```

### Typography
An object with font properties:

| Field          | Type              | Notes                                       |
|----------------|-------------------|---------------------------------------------|
| fontFamily     | string            | Font name                                   |
| fontSize       | Dimension         | e.g., `16px`                                |
| fontWeight     | number            | Numeric: `400`, `700` (quoted strings OK)   |
| lineHeight     | Dimension \| number | Unitless number = multiplier of fontSize  |
| letterSpacing  | Dimension         | e.g., `0.05em`                              |
| fontFeature    | string            | CSS `font-feature-settings` value           |
| fontVariation  | string            | CSS `font-variation-settings` value         |

```yaml
typography:
  h1:
    fontFamily: Public Sans
    fontSize: 48px
    fontWeight: 600
    lineHeight: 1.1
    letterSpacing: -0.02em
```

### Dimension
A string with a unit suffix. Valid units: `px`, `em`, `rem`.

```yaml
rounded:
  sm: 4px
  md: 8px
  full: 9999px
```

### Token References
Cross-reference any token using `{path.to.token}` syntax.

```yaml
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label-md}"   # composite reference allowed in components
    rounded: "{rounded.md}"
```

- References must resolve to a primitive value (except `typography` in components, which may reference a composite group).
- Circular references are a lint error.

---

## Section Order (canonical)

Sections can be omitted; those present must appear in this order:

| # | Section Heading            | Alias                |
|---|----------------------------|----------------------|
| 1 | Overview                   | Brand & Style        |
| 2 | Colors                     | —                    |
| 3 | Typography                 | —                    |
| 4 | Layout                     | Layout & Spacing     |
| 5 | Elevation & Depth          | Elevation            |
| 6 | Shapes                     | —                    |
| 7 | Components                 | —                    |
| 8 | Do's and Don'ts            | —                    |

Duplicate section headings are an error (file is rejected).

---

## Section Descriptions

### Overview
Brand personality, target audience, emotional tone. The agent's fallback
when no specific rule or token applies — keep it concise and evocative.

### Colors
Defines color palettes. `primary` is the minimum required palette.
Common additional palettes: `secondary`, `tertiary`, `neutral`.
Prose should name colors descriptively and explain their semantic role.

### Typography
Defines typography levels (typically 9–15). Common naming:
`display`, `headline-lg/md/sm`, `body-lg/md/sm`, `label-lg/md/sm`, `caption`.

### Layout
Grid model, spacing scale, and rhythm philosophy. Tokens go in `spacing`.

### Elevation & Depth
How visual hierarchy is conveyed — shadows, tonal layers, or borders for flat designs.

### Shapes
Corner radius philosophy. Tokens go in `rounded`.

### Components
Style guidance for UI atoms. Common component keys:

- `button-primary`, `button-primary-hover`, `button-primary-active`
- `button-secondary`, `button-secondary-hover`
- `card`, `card-elevated`
- `input-field`, `input-field-focus`, `input-field-error`
- `chip`, `chip-selected`
- `badge`
- `list-item`, `list-item-hover`

#### Component Property Tokens

| Property        | Type      |
|-----------------|-----------|
| backgroundColor | Color     |
| textColor       | Color     |
| typography      | Typography (composite reference allowed) |
| rounded         | Dimension |
| padding         | Dimension |
| size            | Dimension |
| height          | Dimension |
| width           | Dimension |

Unknown component properties are accepted with a warning.

### Do's and Don'ts
Practical guardrails. Examples:

```markdown
- Do use the primary color only for the single most important action per screen
- Don't mix rounded and sharp corners in the same view
- Do maintain WCAG AA contrast ratios (4.5:1 for normal text)
- Don't use more than two font weights on a single screen
```

---

## Recommended Token Names (Non-Normative)

**Colors:** `primary`, `on-primary`, `primary-container`, `on-primary-container`,
`secondary`, `on-secondary`, `tertiary`, `on-tertiary`, `neutral`, `surface`,
`on-surface`, `surface-variant`, `on-surface-variant`, `outline`, `error`,
`on-error`, `background`, `on-background`

**Typography:** `display`, `headline-lg`, `headline-md`, `headline-sm`,
`body-lg`, `body-md`, `body-sm`, `label-lg`, `label-md`, `label-sm`, `caption`

**Rounded:** `none`, `sm`, `DEFAULT`, `md`, `lg`, `xl`, `full`

**Spacing:** `xs`, `sm`, `md`, `lg`, `xl`, `base`, `gutter`, `margin`

---

## Consumer Behavior for Unknown Content

| Scenario                    | Behavior               |
|-----------------------------|------------------------|
| Unknown section heading     | Preserve; do not error |
| Unknown color token name    | Accept if value valid  |
| Unknown typography name     | Accept as valid        |
| Unknown spacing value       | Accept; store as string if not a valid Dimension |
| Unknown component property  | Accept with warning    |
| Duplicate section heading   | Error — reject file    |

---

## Conversion

DESIGN.md tokens can be imported from / exported to:

- **Figma variables** — map variable collections to token groups
- **tokens.json** (Design Token Community Group format) — use `design-md export --format dtcg`
- **Tailwind config** — use `design-md export --format tailwind`
