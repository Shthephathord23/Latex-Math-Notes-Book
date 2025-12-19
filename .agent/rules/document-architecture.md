---
trigger: always_on
---

# Document Architecture

## Main Structure
- **`main.tex`**: Root document that includes all parts via `\input{}` commands
- **`Preamble/`**: Contains all LaTeX configuration, packages, custom commands, and theorem styles
- **`Document/`**: Organized by mathematical subject areas (`SetTheory/`, `LinearAlgebra/`)

## Content organization pattern
This is how the code should be organized
- Each subject area is a folder that represents a part in the latex document in the main book. It contains a file that includes all the related chapters via ```\input{}``` with chapter folders and uses ```\part{}``` for the part title.
- Each chapter is a folder that represents a chapter in the parent part in the main book. It contains a file that includes all the related sections via ```\input{}``` with section folder or files depending on the content size. It uses ```\chapter{}``` for the chapter title.
- Each section file should contain strictly its content, and if the content is too big to fit in a file, it should be broken into multiple files, all in a folder with a merger file that does ```\input{}``` on what is needed. It uses ```\section{}``` for the section title.

## File Inclusion Strategy
- Each subject uses `\part{}` for major divisions
- Individual topics use `\chapter{}` and `\section{}`
- Files included via `\input{}` for modularity
- Directory structure mirrors mathematical taxonomy

## Mathematical Content Style
- Rigorous axiomatic approach with detailed proofs
- Properties organized as numbered propositions with full proofs
- Examples provide concrete illustrations following abstract definitions
- Remarks explain conceptual connections and intuition
