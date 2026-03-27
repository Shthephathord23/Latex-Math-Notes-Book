---
description: Compiling the pdf
---

# Project Overview

This is a structured mathematical document project written in LaTeX, covering advanced topics (currently in Set Theory and Linear Algebra, but this will expand). The document uses a modular architecture with custom theorem environments, mathematical notation commands, and standardized formatting.

## Build System

### Primary Build Command
Run:
```bash
xelatex -interaction=nonstopmode main.tex
```
If the build fails, read the errors fix them and recompile.

### Build Files
The following files are generated and should not be edited:
- `main.pdf` (output), `main.aux`, `main.log`, `main.toc`, `main.out`, `main.fls`, `main.fdb_latexmk`