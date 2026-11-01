# steering

Read first: `shared/principles.md`, `shared/documents.md`, `shared/steering.md`.

Bring `docs/steering/` in line with the code and with the decisions the user has confirmed, in this conversation or in the documents.

1. When there is nothing to learn from (no code, no project documents, no confirmed decisions), do not write steering from assumptions. Tell the user that a new project's steering is built along with its first requirements, through `/gsdd <idea>`, and stop.
2. Read the existing steering, and study the code as far as the comparison needs.
3. Update steering right away, without asking:
   - Write the choices, patterns, and conventions the code follows throughout, and the decisions the user has confirmed, that steering lacks.
   - Correct the facts the code has since changed. What counts as a fact, and what as a decision, is set out in `shared/steering.md`.
4. Then ask the user about the rest:
   - Where the code departs from a decision in steering, ask which side is right, and change the decision only with their agreement. Fixing the code is not part of this subcommand.
   - The code rarely tells the product's users or the reasons behind a choice: write those as open questions, and ask about the ones that matter.
5. End with a summary: the files changed and why, the open questions, and where the code departs from steering.
