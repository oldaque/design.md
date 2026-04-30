<p align="center">
  <img src="https://em-content.zobj.net/source/apple/391/artist-palette_1f3a8.png" width="100" />
</p>

<h1 align="center">design-md</h1>

<p align="center">
  <strong>Give your AI agents a design system they'll never forget</strong>
</p>

<p align="center">
  <a href="https://github.com/oldaque/design.md/stargazers"><img src="https://img.shields.io/github/stars/oldaque/design.md?style=flat&color=blueviolet" alt="Stars"></a>
  <a href="https://github.com/oldaque/design.md/commits/main"><img src="https://img.shields.io/github/last-commit/oldaque/design.md?style=flat" alt="Last Commit"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/format-DESIGN.md%20alpha-orange?style=flat" alt="Format"></a>
  <a href="https://github.com/google-labs-code/design.md"><img src="https://img.shields.io/badge/spec-google--labs--code-blue?style=flat&logo=google" alt="Spec"></a>
</p>

<p align="center">
  <a href="#the-problem">Problem</a> •
  <a href="#how-it-works">How it works</a> •
  <a href="#install">Install</a> •
  <a href="#usage">Usage</a> •
  <a href="#compatible-agents">Agents</a> •
  <a href="#whats-inside">Inside</a>
</p>

---

An **agent skill** for generating and maintaining [`DESIGN.md`](https://github.com/google-labs-code/design.md) files — Google's open-source format that gives AI coding agents a persistent, structured understanding of your design system.

One file. Any agent. Consistent UI — every single time.

---

## The problem

Every time you start a new chat with an AI coding agent, your brand is gone.

The agent picks random colors. Invents its own spacing. Uses a font it likes. Ignores WCAG contrast rules. Your app looks different in every session.

You've been repeating yourself — pasting your hex codes, re-explaining your design system, correcting the same mistakes — **every. single. session.**

**design-md fixes that.**

---

## How it works

`DESIGN.md` is a plain-text file that lives at the root of your repo. It combines machine-readable design tokens (YAML) with human-readable design rationale (Markdown). When an AI coding agent opens your project, it reads `DESIGN.md` and knows exactly what your brand looks like — colors, typography, spacing, component styles, and the *why* behind every decision.

```
Your brand brief  ──►  design-md skill  ──►  DESIGN.md  ──►  Any AI agent
                                                ↓
                                     Consistent UI, every time
```

This skill handles the hard part: turning your design intent into a spec-compliant `DESIGN.md` file, validating it against the 7 official lint rules (including WCAG AA contrast checks), and keeping it up to date as your design evolves.

---

## Before / After

<table>
<tr>
<td width="50%">

### 😩 Without DESIGN.md

> "Use our brand blue — it's #0057FF... no wait, that's the hover state. The main one is... hold on let me check Figma. Also we use Inter, 16px body, 1.6 line height. And rounded corners, 8px. Oh and buttons have 24px horizontal padding..."

*(every session, forever)*

</td>
<td width="50%">

### ✨ With DESIGN.md

```yaml
colors:
  primary: "#003FCC"
typography:
  body:
    fontFamily: Inter
    fontSize: 16px
    lineHeight: 1.6
components:
  button-primary:
    rounded: "{rounded.md}"
    padding: 12px 24px
```

*Agent already knows. Ships it right.*

</td>
</tr>
</table>

---

## Install

### Claude Code (recommended)

```bash
claude plugin add oldaque/design.md
```

### Any agent via npx

```bash
npx skills add oldaque/design.md
```

### Manual (agent-agnostic)

Copy `SKILL.md` and the `references/` folder into your project's `.agents/skills/` directory — or wherever your agent loads skills from. Works with any tool that reads Markdown skill files.

---

## Usage

Just describe your design intent. The skill handles the rest.

**From a brand color:**
```
/design-md create a DESIGN.md for my app. Primary color is #7C3AED (purple).
Modern SaaS look, Inter font, clean and minimal.
```

**From an existing brand guide or Figma:**
```
/design-md I have these Figma tokens [paste]. Generate a DESIGN.md.
```

**Update an existing file:**
```
/design-md we're rebranding. New primary is #0EA5E9. Update DESIGN.md.
```

**Validate your file:**
```
/design-md lint my DESIGN.md and fix any issues
```

Or run the CLI directly:
```bash
npx design-md lint DESIGN.md
npx design-md export DESIGN.md --format tailwind
npx design-md diff DESIGN.md.old DESIGN.md
```

---

## What gets generated

A `DESIGN.md` at your repo root with:

| Section | What it defines |
|---|---|
| **YAML frontmatter** | Machine-readable tokens: colors, typography, spacing, rounded corners, component styles |
| **Overview** | Brand personality and emotional tone — the agent's fallback for any design decision |
| **Colors** | Palette with semantic roles and rationale |
| **Typography** | Font scale with roles (display, headline, body, label, caption) |
| **Layout** | Grid model and spacing philosophy |
| **Elevation & Depth** | Shadow strategy or flat-design alternatives |
| **Shapes** | Corner radius philosophy |
| **Components** | Button, card, input, chip, badge — with hover/active variants |
| **Do's and Don'ts** | Guardrails against the most common AI design mistakes |

The output is validated against **7 lint rules** before delivery:

- 🔴 `broken-ref` — all `{token.references}` resolve (error)
- 🟡 `missing-primary` — `colors.primary` is defined (warning)
- 🟡 `contrast-check` — WCAG AA 4.5:1 minimum on all components (warning)
- 🟡 `orphaned-tokens` — no defined token goes unused (warning)
- 🟡 `section-order` — sections in canonical order (warning)
- 🟡 `missing-typography` — typography exists when colors do (warning)
- 🔵 `missing-sections` — spacing and rounded sections present (info)

---

## Compatible agents

`DESIGN.md` is an **open, agent-agnostic format**. This skill works anywhere:

| Agent | Install method |
|---|---|
| Claude Code | `claude plugin add oldaque/design.md` |
| Cursor | Copy to `.cursor/skills/` |
| Kiro | Copy to `.kiro/skills/` |
| Codex | `npx skills add oldaque/design.md` |
| Any agent | Copy `SKILL.md` + `references/` to your skills folder |

The generated `DESIGN.md` is consumed by any agent that reads your repo — no plugins required on the consumer side.

---

## What's inside

```
SKILL.md                     — Main skill: workflow, validation, tips
references/
  spec.md                    — Full token schema and section order
  linting-rules.md           — All 7 lint rules with severity and fixes
  cli.md                     — design-md CLI command reference
  examples.md                — Two complete production examples
```

Reference files are loaded on demand — they don't bloat your context unless needed.

---

## Based on the open standard

This skill implements the [`DESIGN.md`](https://github.com/google-labs-code/design.md) specification open-sourced by Google Labs. The format is compatible with:

- **Figma** variables (import/export)
- **Tailwind CSS** theme config (`npx design-md export --format tailwind`)
- **Design Token Community Group** / W3C format (`--format dtcg`)
- **tokens.json** (Style Dictionary, Tokens Studio)

---

## Star this repo ⭐

If this saved you from pasting hex codes into a chatbox one more time, consider starring the repo — it helps other developers find it and keeps the spec up to date.

**[⭐ Star on GitHub](https://github.com/oldaque/design.md)**

Found a bug or want to contribute? [Open an issue](https://github.com/oldaque/design.md/issues) or submit a PR. The `references/` files are the best place to start — they're plain Markdown.

---

<p align="center">
  <sub>Built on <a href="https://github.com/google-labs-code/design.md">google-labs-code/design.md</a> · Format version: alpha · Works with Claude Code, Cursor, Kiro, Codex, and beyond</sub>
</p>
