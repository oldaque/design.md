# design-md CLI Reference

> Source: [google-labs-code/design.md](https://github.com/google-labs-code/design.md) packages/cli/

The `design-md` CLI is a Node.js tool for validating, comparing, and exporting
DESIGN.md files. It is optional — agents can generate and read DESIGN.md without
the CLI — but it provides structured, machine-readable output that agents can act on.

## Installation

```sh
npm install -g design-md
# or run without installing:
npx design-md <command>
```

---

## Commands

### `lint` — Validate a DESIGN.md file

```sh
design-md lint <file> [--format json|text]
```

| Argument | Description |
|----------|-------------|
| `file`   | Path to DESIGN.md. Use `-` to read from stdin. |
| `--format` | Output format: `json` (default) or `text`. |

**Exit code:** `1` if any `error` findings exist; `0` otherwise.

**JSON output shape:**
```json
{
  "findings": [
    {
      "ruleId": "broken-ref",
      "severity": "error",
      "message": "Reference {colors.primay} does not resolve to any defined token."
    }
  ],
  "summary": {
    "errors": 1,
    "warnings": 2,
    "infos": 1
  }
}
```

**Example — validate and check for errors:**
```sh
npx design-md lint DESIGN.md --format text
```

---

### `diff` — Compare two DESIGN.md files

```sh
design-md diff <before> <after> [--format json|text]
```

| Argument  | Description                      |
|-----------|----------------------------------|
| `before`  | Path to the "before" DESIGN.md   |
| `after`   | Path to the "after" DESIGN.md    |
| `--format` | Output format: `json` or `text` |

Reports which tokens were added, removed, or changed across colors, typography,
rounded, and spacing. Also compares lint finding counts.

**JSON output shape:**
```json
{
  "tokens": {
    "colors": { "added": [...], "removed": [...], "changed": [...] },
    "typography": { ... },
    "rounded": { ... },
    "spacing": { ... }
  },
  "findings": {
    "before": { "errors": 0, "warnings": 2, "infos": 1 },
    "after":  { "errors": 0, "warnings": 1, "infos": 1 },
    "delta":  { "errors": 0, "warnings": -1, "infos": 0 }
  }
}
```

**Example — show what changed in a PR:**
```sh
npx design-md diff DESIGN.md.old DESIGN.md
```

---

### `export` — Convert tokens to another format

```sh
design-md export <file> --format tailwind|dtcg
```

| Argument   | Description                                |
|------------|--------------------------------------------|
| `file`     | Path to DESIGN.md. Use `-` for stdin.     |
| `--format` | Target format: `tailwind` or `dtcg`.      |

**`tailwind`** — Outputs a Tailwind CSS theme extension object. Paste it into
`tailwind.config.js` under `theme.extend`.

**`dtcg`** — Outputs a Design Token Community Group (W3C draft) token file.
Compatible with Figma variables, Style Dictionary, and Tokens Studio.

**Example — generate Tailwind config:**
```sh
npx design-md export DESIGN.md --format tailwind > tailwind-tokens.json
```

**Example — export to Figma-compatible DTCG format:**
```sh
npx design-md export DESIGN.md --format dtcg > tokens.json
```

---

### `spec` — Print the machine-readable specification

```sh
design-md spec
```

Prints the full token schema and section order as structured JSON. Useful for
agents that want to validate their output programmatically without running lint.

---

## Stdin / Stdout pipeline

All commands accept `-` as the file path to read from stdin:

```sh
cat DESIGN.md | npx design-md lint -
echo $DESIGN_MD_CONTENT | npx design-md export - --format tailwind
```

---

## Integration patterns

**Pre-commit hook** — fail if DESIGN.md has errors:
```sh
npx design-md lint DESIGN.md
```

**CI check** — validate on pull request:
```yaml
- run: npx design-md lint DESIGN.md --format text
```

**Agent workflow** — lint after generating, parse JSON findings, fix errors:
```sh
RESULT=$(npx design-md lint DESIGN.md)
ERRORS=$(echo $RESULT | jq '.summary.errors')
if [ "$ERRORS" -gt 0 ]; then
  echo "$RESULT" | jq '.findings[] | select(.severity == "error")'
fi
```
