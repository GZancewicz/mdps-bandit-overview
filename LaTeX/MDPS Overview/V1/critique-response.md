# Critique Response

## Changes Accepted and Implemented

### 1. Fix section cross-references with `\label`/`\ref` (Critique 1)
Adding the Introduction shifted all section numbers by one, so every hardcoded `Section~1`, `Section~2`, etc. in the body text is now wrong. Switching to `\label`/`\ref` solves this permanently.

### 2. Add conditions to regret/complexity claims (Critique 1)
Three specific softening edits:
- Thompson Sampling: add "under standard regularity conditions on the prior"
- UCB1 matching Lai-Robbins: add "for bounded reward distributions"
- PSPACE-complete for POMDPs: clarify this applies to finite-horizon exact planning

### 3. Glossary consistency (Critique 1)
Some entries are full sentences with equations, others are terse. A uniform pass to give each entry the same structure (definition + equation if applicable + section ref) improves readability.

### 4. Add `\usepackage{lmodern}` (Critique 1)
Low-effort fix that prevents subtle font issues with the T2A/Cyrillic encoding loaded for the Markov citation.

### 5. Add explicit pedagogical intent statement (Critique 2, scoped down)
Add one sentence to the Introduction making it explicit: "This paper is pedagogical in intent; it presents no new results."

## Changes Declined

### Move examples/code to appendices (Critique 1)
The worked examples and Python code are a core feature of the paper, as stated in the title ("Worked Examples") and the abstract. Moving them to appendices would undermine the paper's purpose.

### Rename Beta parameters to avoid notation collision (Critique 1)
We already added disambiguating footnotes and parentheticals for α, β. Renaming to a, b would collide with actions and Bellman's notation. The current approach (footnotes + table notes) is sufficient.

### Add a "When MDPs Are the Wrong Abstraction" section (Critique 2)
This paper is an overview of what these models *are*, not a critique of when they fail. Section 7 ("Beyond the Scope") already acknowledges limitations. A failure-mode analysis is a different paper.

### Missing citations (Critique 2)
This critique appears to have been written against an earlier draft or without noticing the bibliography. We already cite Puterman, Gittins, Whittle, Bellman, Lai-Robbins, and 18 other references.

### Proof sketches / convergence conditions (Critique 2)
This is a tutorial, not a research paper. Adding proof sketches would change the scope.

### Actions-as-nodes in diagrams (Critique 2)
Our MDP diagrams already label action nodes with dotted rectangles and state nodes with solid circles, with an explicit caption explaining the convention. This is a standard decision-diagram style.
