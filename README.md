# design-md skill

An agent skill for generating and maintaining
[DESIGN.md](https://github.com/google-labs-code/design.md) files — Google's
open-source format for describing a visual identity to AI coding agents.

## What this skill does

- Generates complete, spec-compliant `DESIGN.md` files from a design brief,
  a color palette, an existing brand guide, or any combination thereof
- Updates existing `DESIGN.md` files when tokens change
- Validates output against the seven official lint rules
- Translates from Figma variables, `tokens.json`, or Tailwind config into DESIGN.md format

## Structure

```
SKILL.md                     — Main skill (instructions + workflow)
references/
  spec.md                    — Full token schema and section reference
  linting-rules.md           — All 7 lint rules with severity and fix guidance
  cli.md                     — design-md CLI command reference
  examples.md                — Two complete production examples
```

## Specification

DESIGN.md format version: **alpha**  
Upstream: [google-labs-code/design.md](https://github.com/google-labs-code/design.md)  
Stitch docs: [stitch.withgoogle.com/docs/design-md](https://stitch.withgoogle.com/docs/design-md/overview/)

## Agent compatibility

This skill and the DESIGN.md format are agent-agnostic.  
Compatible with: Claude Code, Cursor, Kiro, and any agent that reads files.
