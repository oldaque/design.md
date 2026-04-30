---
name: design-md
description: >
  Generate and maintain DESIGN.md files — the open-source format for describing
  a visual identity to AI coding agents. Use this skill whenever the user wants
  to create a design system, document brand tokens, generate or update a
  DESIGN.md file, define colors/typography/spacing/components for a project,
  or ensure consistent UI generation across AI tools (Claude Code, Cursor, Kiro,
  etc.). Also trigger when the user mentions "design tokens", "brand guidelines",
  "color palette", "typography scale", or "component styles" in the context of
  a software project.
---

# DESIGN.md Skill

DESIGN.md is a plain-text, open-format specification that gives AI coding agents
a persistent, structured understanding of a design system. A single file defines
the visual identity of a brand — colors, typography, spacing, shapes, and
component styles — so every agent generates UIs that match the brand without
re-inventing it each session.

The format is agent-agnostic: Claude Code, Cursor, Kiro, and any other tool that
reads DESIGN.md will follow the same design system.

Read `references/spec.md` for the full field reference.  
Read `references/linting-rules.md` for validation guidance.  
Read `references/cli.md` for the `design-md` CLI commands.

---

## When to create vs. update

- **Create**: no `DESIGN.md` exists and the user wants consistent UI generation.
- **Update**: tokens change (new color, font swap), a section is missing, or lint
  violations need fixing.
- **Import**: the user has Figma variables, `tokens.json`, or Tailwind config —
  translate those values into DESIGN.md format.

---

## Workflow

### 1 — Gather design intent

Before writing tokens, understand:

- **Brand personality** — playful or professional? dense or airy? bold or subtle?
- **Primary color** — even one hex is enough to derive a full palette.
- **Typeface** — system font or a Google Font / custom typeface?
- **Target surface** — web, mobile, or both? dark mode needed?
- **Existing assets** — Figma file, brand guide PDF, tokens.json, or Tailwind config?

If the user provides a reference image or screenshot, extract dominant colors and
infer the aesthetic. Ask only what you genuinely need; a minimal DESIGN.md is
better than a stalled conversation.

### 2 — Draft the YAML frontmatter (tokens)

Always include these fields in this order inside the `---` delimiters:

```yaml
---
version: alpha
name: <Project Name>
description: <one-line summary>   # optional but helpful
colors: ...
typography: ...
rounded: ...
spacing: ...
components: ...
---
```

Token rules to follow:

- **Colors** must be hex strings starting with `#` (e.g., `"#1A1C1E"`).
  Name palettes `primary`, `secondary`, `tertiary`, `neutral` when possible.
  For rich palettes also add `on-primary`, `primary-container`, etc. (Material
  You convention is widely understood).
- **Typography** objects require at minimum `fontFamily`, `fontSize`, and
  `fontWeight`. Add `lineHeight` and `letterSpacing` for precision.
  fontWeight must be a numeric value (e.g., `400`, `700`).
- **Dimensions** use `px`, `em`, or `rem` units.
- **Token references** use `{path.to.token}` — e.g., `{colors.primary}`.
  Components may reference composite typography groups:
  `typography: "{typography.body-md}"`.
- **Components** map identifiers to sub-token groups. Valid sub-tokens:
  `backgroundColor`, `textColor`, `typography`, `rounded`, `padding`,
  `size`, `height`, `width`. Add variant keys for states:
  `button-primary`, `button-primary-hover`, `button-primary-active`.

### 3 — Write the Markdown body

Sections must appear in this canonical order (omit irrelevant ones):

1. **Overview** (also "Brand & Style") — brand personality, emotional tone,
   target audience. This is the agent's fallback when no specific token applies.
2. **Colors** — prose rationale for each palette. Name colors descriptively
   ("Midnight Forest Green") and tie them to their token name (`primary`).
3. **Typography** — explain the typeface choice and each scale level's role.
4. **Layout** (also "Layout & Spacing") — grid system, spacing philosophy.
5. **Elevation & Depth** (also "Elevation") — shadow strategy or flat-design
   alternatives (borders, tonal contrast).
6. **Shapes** — corner radius philosophy and how it varies across components.
7. **Components** — style guidance for buttons, cards, inputs, chips, etc.
8. **Do's and Don'ts** — guardrails for common misuse patterns.

Every section uses `##` headings. An optional `# Title` may precede all sections.

The prose is the agent's *why*: it explains the reasoning behind token values so
agents make correct decisions in cases the tokens don't explicitly cover.

### 4 — Validate before delivering

Check the output against the seven lint rules (see `references/linting-rules.md`):

| Rule              | Severity | What to check                                     |
|-------------------|----------|---------------------------------------------------|
| `broken-ref`      | error    | All `{path.to.token}` references resolve          |
| `missing-primary` | warning  | `colors.primary` is defined                       |
| `contrast-check`  | warning  | Component `textColor` / `backgroundColor` pairs ≥ 4.5:1 WCAG AA |
| `orphaned-tokens` | warning  | Every token is referenced by at least one component|
| `section-order`   | warning  | Sections appear in canonical order                |
| `missing-typography` | warning | Typography tokens exist when colors do           |
| `missing-sections` | info    | `spacing` and `rounded` sections are present     |

Fix errors before delivering. Address warnings where practical. Info findings
are informational — note them to the user if relevant.

If the CLI is available, run:
```sh
npx design-md lint DESIGN.md
```

### 5 — Place the file

`DESIGN.md` lives at the root of the repository so agents discover it
automatically. If the project already has one, update it in place.

---

## Minimal example

```markdown
---
version: alpha
name: Acme App
colors:
  primary: "#1A6B4A"
  on-primary: "#FFFFFF"
  neutral: "#F5F5F5"
typography:
  h1:
    fontFamily: Inter
    fontSize: 32px
    fontWeight: 700
    lineHeight: 1.2
  body:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.6
rounded:
  sm: 4px
  md: 8px
  lg: 16px
spacing:
  sm: 8px
  md: 16px
  lg: 32px
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.md}"
    padding: "{spacing.md}"
---

## Overview

A clean, trustworthy interface for productivity tools. Professional tone with
generous whitespace; no decorative flourishes.

## Colors

- **Primary (#1A6B4A):** Deep forest green for all primary actions and CTAs.
- **On-Primary (#FFFFFF):** White for text and icons on green surfaces.
- **Neutral (#F5F5F5):** Off-white page backgrounds.

## Typography

**Inter** across all levels — legible at every size, neutral enough for
business contexts.

## Do's and Don'ts

- Do use primary color only for the single most important action per screen.
- Don't use more than two font weights per view.
```

---

## Tips for quality output

- Derive a full token set from even a single brand color — generate light/dark
  variants, surface tones, and semantic roles (`on-primary`, `error`, etc.).
- When generating component tokens, cover at minimum:
  `button-primary`, `button-primary-hover`, `card`, `input-field`.
- Prefer `{token.references}` over literal hex values in components — it keeps
  the file maintainable and lets agents trace reasoning.
- Keep prose concise. Two or three sentences per section is usually enough.
  The tokens are the normative values; prose is context, not a style guide essay.
