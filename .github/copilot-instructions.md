# LaTeX Mathematical Document Instructions

## Project Overview
This is a structured mathematical document project written in LaTeX, covering advanced topics (currently in Set Theory and Linear Algebra, but this will expand). The document uses a modular architecture with custom theorem environments, mathematical notation commands, and standardized formatting.

## Document Architecture

### Main Structure
- **`main.tex`**: Root document that includes all parts via `\input{}` commands
- **`Preamble/`**: Contains all LaTeX configuration, packages, custom commands, and theorem styles
- **`Document/`**: Organized by mathematical subject areas (`SetTheory/`, `LinearAlgebra/`)
- Each subject area is a folder that should be a chapter

### Critical Dependencies
The build process requires the following order:
1. `Preamble/Packets/Packets.tex` - Core LaTeX packages (ams*, physics, xparse, etc.)
2. `Preamble/Commands/Commands.tex` - Custom mathematical notation commands
3. `Preamble/Theorems/Theorems.tex` - Theorem environment definitions with custom styling

## Build System

### Primary Build Command
Use the VS Code task: "Build LaTeX Document" or run:
```bash
pdflatex -interaction=nonstopmode main.tex
```

### Build Files
The following files are generated and should not be edited:
- `main.pdf` (output), `main.aux`, `main.log`, `main.toc`, `main.out`, `main.fls`, `main.fdb_latexmk`

## Mathematical Notation System

### Custom Quantifier Commands
The project uses sophisticated quantifier notation via `\MakeQuantifier`:
- `\Forall{variable}{predicate}` - Universal quantification
- `\Exists{variable}{predicate}` - Existential quantification
- Optional modifiers: `p` (parentheses), `:` (colon separator)
- Examples: `\Forall p{x \in A}{\Exists p{y}{P(x,y)}}`, `\Exists :{x \in \bb{R}}{x > 0}`

### Binary Relations Commands
- `\Domain{R}`, `\Range{R}`, `\Codomain{R}` - Domain/range notation
- `\ConverseRel{R}` or `\ConverseRel*{R}` - Converse relation (with/without parentheses)
- `\ComplementRel{R}` - Complement relation
- `\Composition{S}{R}` - Relation composition (S ∘ R)
- `\Restriction{R}[A][B]` - Restriction notation with optional domain/codomain
- `\DirectImage{R}{A}`, `\InverseImage{R}{B}` - Image operations
- `\UniversalRel{A}[B]` - Universal relation notation

### General Math Commands
- `\bb{R}`, `\bb{Z}`, `\bb{C}` - Blackboard bold via `\bb{}`
- `\defeq`, `\eqdef` - Definition equality symbols
- Custom inequality: `\le` (redefined as `<`), `\ge` (redefined as `>`)

## Theorem Environment System

### Available Environments
All environments share numbering via `[num]` and use custom break styling:
- `definition`, `proposition`, `theorem`, `corollary` - Main mathematical statements
- `example` - Examples with definition-style formatting  
- `remark` - Unnumbered remarks
- All theorem environments automatically indent content by 2em

### Proof Environment
- Standard `proof` environment with custom QED symbol
- Ends with "QED" instead of standard symbol

## Content Organization Patterns

### File Inclusion Strategy
- Each subject uses `\part{}` for major divisions
- Individual topics use `\chapter{}` and `\section{}`
- Files included via `\input{}` for modularity
- Directory structure mirrors mathematical taxonomy

### Mathematical Content Style
- Rigorous axiomatic approach with detailed proofs
- Properties organized as numbered propositions with full proofs
- Examples provide concrete illustrations following abstract definitions
- Remarks explain conceptual connections and intuition

## Development Workflow

### Adding New Mathematical Content
1. Create appropriately named `.tex` file in relevant `Document/` subdirectory
2. Add `\input{}` command to parent file
3. Use established theorem environments and custom notation
4. Build document to verify LaTeX compilation
5. Follow existing proof structure and notation patterns

### Adding New Commands
1. Add to appropriate file in `Preamble/Commands/`
2. Use `\NewDocumentCommand{}` with xparse syntax for flexibility
3. Follow existing patterns for optional arguments and boolean toggles
4. Test compilation after adding commands

### Common Notation Patterns
- Set membership: `\Forall :{x \in A}{P(x)}`
- Existence: `\Exists :{y \in B}{Q(y)}`
- Binary relations: `\Composition{S}{R}`, `\ConverseRel{R}`
- Function application: `\DirectImage{f}{A}`, `\InverseImage{f}{B}`

Always maintain mathematical rigor and follow the established proof style with detailed justifications for each step.
