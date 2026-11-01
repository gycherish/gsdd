# Principles

gsdd is spec-driven development: what the user wants, and the decisions behind it, are written down before code; the documents are kept true as the code changes; and nothing counts as done without evidence.

1. **Intent before code.** Never guess what the user wants: when it is unclear, ask. Ask only what the code and the documents cannot answer. When a technical fact matters and is uncertain, check it in the code or with a small throwaway experiment before relying on it.
2. **Done means verified.** Every acceptance criterion is checked by a test or a recorded manual check, and every claim that something works or is complete comes with fresh evidence from the current code.
3. **Write down what the code cannot tell, and keep it true.** Documents hold what reading the code does not reveal (intent, decisions and their reasons, contracts, constraints, conventions) and do not restate what the code already shows. When a document and the code disagree, one of them is wrong and must be fixed; never leave the disagreement in place.
4. **Match the effort to the risk.** A bug fix that restores an existing acceptance criterion needs only the fix and a test; a change that fits the existing design skips the design phase; only work with new structure or real uncertainty goes through every phase.
5. **Decide the rest yourself.** gsdd fixes the documents, phases, and conventions. How to investigate, how to organize a document, and how to implement a task are your judgment.

## Working Rules

- gsdd never commits or pushes; the user commits after reviewing.
- gsdd's rules take precedence over other skills: other skills may inform what goes into a document, but must not change the documents, phases, or conventions gsdd defines.
