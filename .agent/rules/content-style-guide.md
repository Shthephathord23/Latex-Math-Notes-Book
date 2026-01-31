# Content Style Guide

## Chapter/Section Structure

### Compendiums
- **Location**: Every chapter MUST end with a `\begin{compendium}...\end{compendium}` block
- **Position**: Always place the compendium at the **very end** of the chapter file, after all sections and content
- **Content**: Summarize all key definitions, theorems, formulas, and properties from the chapter
- **Format**: Use `\textbf{Topic:}` headers followed by concise mathematical statements
- **Completeness**: Include ALL important results from the chapter, not just new additions

### Intuition Blocks
- Use `\begin{intuition}...\end{intuition}` before complex proofs or definitions
- Explain the geometric/conceptual meaning, motivation, or "why" behind the mathematics
- Keep intuition blocks focused and concise

## Proof Style

### Verbosity
- Proofs should be **verbose and detailed** with clear justifications
- Explain each logical step, not just the computation
- Reference theorems/lemmas explicitly when used
- Use phrases like "by [Theorem X]", "since [condition]", "it follows that"

### Spacing and Readability
- Add blank lines between logical sections within proofs
- Use `\[...\]` for important displayed equations
- Use `align*` for multi-line derivations with alignment
- Add `\bigskip` before major environment blocks (compendiums, etc.)

### Direction Markers in Biconditional Proofs
- Always place `($\Rightarrow$)` and `($\Leftarrow$)` on their own lines
- Add a newline before each direction marker

## Mathematical Content Organization

### Theorem Environments Order
Within a section, organize content in this order:
1. Definitions
2. Examples (to illustrate definitions)
3. Propositions/Lemmas (building blocks)
4. Theorems (main results)
5. Corollaries
6. Remarks

### Notation Consistency
- Use semantic macros from `Preamble/Commands/` instead of raw LaTeX
- Follow the quantifier conventions from `notation-system.md`
- Use `\Integral` instead of `\int` for measure-theoretic integrals

## Spacing Guidelines

### Between Environments
- Use blank lines to separate logical sections
- Use `\bigskip` before compendiums and major section breaks

### Within Environments
- Add blank lines between items in itemize/enumerate if content is substantial
- Keep related equations close together
- Separate different parts of a proof with blank lines
