---
description: Bibliography management and citation conventions
---

# Bibliography Management

## File Structure

Each document folder has its own `references.bib`:

```
Math/
├── Document/
│   ├── SetTheory/
│   │   └── references.bib
│   ├── LinearAlgebra/
│   │   └── references.bib
│   └── UniBuc FMI/
│       └── references.bib
```

## How It Works

The `\bibliography{}` command in `main.tex` loads all local .bib files:
```latex
\bibliography{Document/SetTheory/references,Document/LinearAlgebra/references,Document/UniBuc FMI/references}
```

## Adding New References

1. Add the entry to the appropriate local `references.bib`
2. That's it!

## BibTeX Entry Format

```bibtex
@book{authorYEARkeyword,
  title={Full Title},
  author={Last, First and Last2, First2},
  year={YYYY},
  edition={Nth},
  publisher={Publisher Name}
}
```

Use consistent key format: `authorYEARkeyword` (e.g., `rudin1987real`)

## Adding a New Subject Folder

1. Create `references.bib` in the new folder
2. Add the path to `\bibliography{}` in `main.tex`:
   ```latex
   \bibliography{...,Document/NewFolder/references}
   ```

## main.tex Configuration

```latex
\nocite{*}  % Include all entries even without explicit \cite{}
\bibliographystyle{alpha}
\bibliography{Document/SetTheory/references,Document/LinearAlgebra/references,Document/UniBuc FMI/references}
```

## Common Reference Types

- `@book` - Books and textbooks
- `@article` - Journal articles  
- `@inproceedings` - Conference papers
- `@phdthesis` - PhD dissertations
- `@misc` - Online resources, lecture notes