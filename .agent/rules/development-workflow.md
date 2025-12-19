---
trigger: always_on
---

# Development Workflow

## Adding New Mathematical Content
1. Create appropriately named `.tex` file in relevant `Document/` subdirectory
2. Add `\input{}` command to parent file
3. Use established theorem environments and custom notation
4. Build document to verify LaTeX compilation
5. Follow existing proof structure and notation patterns

## Adding New Commands
1. Add to appropriate file in `Preamble/Commands/`
2. Use `\NewDocumentCommand{}` with xparse syntax for flexibility
3. Follow existing patterns for optional arguments and boolean toggles
4. Test compilation after adding commands

## Common Notation Patterns
- Set membership: `\Forall :{x \in A}{P(x)}`
- Existence: `\Exists :{y \in B}{Q(y)}`
- Binary relations: `\Composition{S}{R}`, `\ConverseRel{R}`
- Function application: `\DirectImage{f}{A}`, `\InverseImage{f}{B}`

Always maintain mathematical rigor and follow the established proof style with detailed justifications for each step.
