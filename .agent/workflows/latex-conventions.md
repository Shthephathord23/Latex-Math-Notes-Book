---
description: LaTeX conventions for math documents
---

# LaTeX Math Document Conventions

## Text in Math Mode

When writing text within math commands or macros, use `\mathrm{}` instead of `\text{}`:

- **Use**: `\mathrm{a.e.}` 
- **Avoid**: `\text{a.e.}`

**Reason**: `\text{}` inherits the surrounding text style, so if used within a definition environment (italic), the text will also be italic. `\mathrm{}` always produces upright roman text.

## Inequality Symbols

Per `Generic.tex`:
- `\le` produces `<` (strict less than)
- `\ge` produces `>` (strict greater than)
- Use `\leq` for ≤ (less than or equal)
- Use `\geq` for ≥ (greater than or equal)

## Custom Macros Location

- Generic: `Preamble/Commands/Generic.tex`
- Logic: `Preamble/Commands/Logic.tex`
- Set theory: `Preamble/Commands/SetTheory.tex`
- Binary relations: `Preamble/Commands/BinaryRelations.tex`
- Topology: `Preamble/Commands/Topology.tex`
- Measure theory: `Preamble/Commands/MeasureTheory.tex`
