---
trigger: always_on
glob: "**/*.tex"
description: "LaTeX code style guidelines for the project"
---

# LaTeX Code Style Guide

## General Formatting
- **Indentation**: Use 4 spaces for indentation.
- **Line Length**: Try to keep lines under 100 characters. Break long lines at logical points (e.g., after punctuation or operators).
- **Whitespace**: Use blank lines to separate logical sections of the code (e.g., between theorem environments).

## Mathematical Environments
- **Equations**: 
  - Use `\[ ... \]` for simple display math.
  - Use `align*` for multi-line equations or when alignment is needed.
  - Avoid `$$ ... $$`.
- **Theorems**: Use the project-specific environments: `definition`, `theorem`, `proposition`, `corollary`, `lemma`, `remark`, `example`.
- **Proofs**: Use the `proof` environment.
  - Always place a newline before `($\Rightarrow$)` and `($\Leftarrow$)` symbols in proofs to separate the directions clearly.

## Custom Commands
- **Quantifiers**: Always use `\Forall` and `\Exists` instead of `\forall` and `\exists`.
- **Relations**: Use semantic commands like `\Composition`, `\ConverseRel`, `\DirectImage`, `\InverseImage`.
- **Sets**: Use `\Set{...}` or `\left\{ ... \right\}` for set notation.

## Referencing
- **Labels**: Use descriptive prefixes for labels:
  - `thm:` for theorems
  - `prop:` for propositions
  - `def:` for definitions
  - `lem:` for lemmas
  - `cor:` for corollaries
  - `eq:` for equations
  - `sec:` for sections
- **References**: Use `\ref{...}` or `\eqref{...}` for equations.

## Comments
- Use `%` to comment out code or add explanatory notes.
- Add comments to explain complex LaTeX constructions or "magic" formatting.
