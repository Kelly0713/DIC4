## ADDED Requirements

### Requirement: Compilable IEEE conference Overleaf draft in Traditional Chinese
The `overleaf_draft/` directory and `overleaf_ieee_draft.zip` package SHALL provide a complete IEEE conference LaTeX draft using `\documentclass[conference]{IEEEtran}` configured for Traditional Chinese (`xeCJK` / `ctex` compatible with Overleaf XeLaTeX via `latexmkrc`), preserving all core sections, citations (`[1]`–`[12]`), tables, architecture/flowchart TikZ figures, and Algorithm 1 pseudocode from `物聯網_Final_Project_Proposal.pdf`.

#### Scenario: Uploading package to Overleaf
- **WHEN** a user uploads `overleaf_ieee_draft.zip` to Overleaf as a new project
- **THEN** Overleaf automatically selects XeLaTeX (via `latexmkrc`) and compiles `main.tex` with `references.bib` into a two-column IEEE conference PDF containing placeholder author/affiliation fields ready for user editing
