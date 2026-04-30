# DESIGN.md Linting Rules

> Source: [google-labs-code/design.md](https://github.com/google-labs-code/design.md) packages/cli/src/linter/linter/rules/

Run validation with:
```sh
npx design-md lint DESIGN.md
npx design-md lint DESIGN.md --format text   # human-readable output
```

Output is JSON by default:
```json
{
  "findings": [
    { "ruleId": "broken-ref", "severity": "error", "message": "..." }
  ],
  "summary": { "errors": 1, "warnings": 2, "infos": 1 }
}
```

Exit code is `1` when any `error` findings exist.

---

## Rules

### `broken-ref` · severity: **error**

Checks that every `{path.to.token}` reference resolves to a defined token and
that no circular references exist. Also warns when a component uses a property
name that is not a recognized sub-token.

**How to fix:**
- Confirm the referenced path exists in the YAML frontmatter.
- Check for typos (e.g., `{colors.primay}` → `{colors.primary}`).
- Recognized component sub-tokens: `backgroundColor`, `textColor`,
  `typography`, `rounded`, `padding`, `size`, `height`, `width`.

---

### `missing-primary` · severity: **warning**

Fires when a `colors` section is present but no `colors.primary` token is
defined. Without a primary color the agent will auto-generate key colors,
reducing control over the palette.

**How to fix:**
```yaml
colors:
  primary: "#1A6B4A"
```

---

### `contrast-check` · severity: **warning**

For each component that defines both `backgroundColor` and `textColor`,
computes the WCAG contrast ratio. Warns when the ratio falls below **4.5:1**
(WCAG AA for normal text).

**How to fix:** Adjust either the background or text color to achieve sufficient
contrast. Tools: [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/),
[Coolors Contrast Checker](https://coolors.co/contrast-checker).

Note: this rule only fires when both properties are literal hex values (not token
references that the linter cannot fully resolve at parse time).

---

### `orphaned-tokens` · severity: **warning**

Flags tokens defined in `colors`, `typography`, `rounded`, or `spacing` that
are never referenced by any component. Orphaned tokens suggest the design system
defines more than agents will ever use.

**How to fix:** Either remove the unused token or add a component that references
it. It is acceptable to keep tokens that are intentionally available for prose
reference but not yet assigned to components — in that case, suppress this
warning per-token if the CLI supports it.

---

### `section-order` · severity: **warning**

Checks that sections appear in the canonical order:

1. Overview  
2. Colors  
3. Typography  
4. Layout  
5. Elevation & Depth  
6. Shapes  
7. Components  
8. Do's and Don'ts  

**How to fix:** Reorder the `##` sections to match the sequence above. Sections
can be omitted; only those present need to be in order.

Auto-fix available via CLI:
```sh
npx design-md fix DESIGN.md
```

---

### `missing-typography` · severity: **warning**

When a `colors` section is defined (indicating an intentional design system),
but no `typography` tokens exist, this rule fires. Agents will fall back to
default font choices, reducing typographic consistency.

**How to fix:** Add at minimum one typography level:
```yaml
typography:
  body:
    fontFamily: Inter
    fontSize: 16px
    fontWeight: 400
    lineHeight: 1.6
```

---

### `missing-sections` · severity: **info**

Informational notice when the optional `spacing` or `rounded` sections are
absent. Agents can still generate UIs, but spacing and shape decisions will be
inconsistent.

**How to fix:** Add the missing sections with at least a base scale:
```yaml
rounded:
  sm: 4px
  md: 8px
  lg: 16px
spacing:
  sm: 8px
  md: 16px
  lg: 32px
```

---

### `token-summary` · severity: **info**

Always fires — emits a summary of how many tokens are defined
(e.g., "Design system defines 12 colors, 6 typography levels, 3 rounded values,
5 spacing values, 4 components."). Useful for verifying the file was parsed
correctly.

---

## Severity Reference

| Severity | Meaning                                         | CLI exit code |
|----------|-------------------------------------------------|---------------|
| error    | Structural problem that agents cannot handle    | 1 (non-zero)  |
| warning  | Likely to cause inconsistent or suboptimal UI   | 0             |
| info     | Informational — no action required              | 0             |
