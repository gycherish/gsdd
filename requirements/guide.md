# requirements

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`, `shared/capabilities.md`, `requirements/ears.md`. Before writing steering, also read `shared/steering.md`.

The request is the arguments, or what the user has said in this conversation. With no request, ask what the user wants; when a capability is in the `requirements` phase, offer to continue it. When the user confirms requirements that are already written, go straight to Confirm.

This subcommand writes documents only; it never changes code.

## 1. Understand the Situation

- List the capabilities and their phases (every `spec.json` under `docs/specs/`), and read the steering.
- Read the material the user points to, such as a requirements document.
- Read the requirements of the capabilities the request may touch, and look at the code as far as needed to know what exists.
- A project with no code, no steering, and no capabilities is new: this is its first requirements analysis.

## 2. Decide What the Request Is

- **A change to existing capabilities**: it changes their requirements, or only their design, in which case it starts at `design` (`shared/phases.md`). When unsure whether a request changes an existing capability or needs a new one, ask.
- **New capabilities**: new parts of the system, by the rules in `shared/capabilities.md`. A part that exists in the code but has no capability yet becomes one now: write only the requirements this change needs (the new behavior, and the existing behavior it touches), and leave the rest to later changes.
- **A bug**: behavior that breaks what the requirements say. When the requirements plainly do not cover the case, it is a requirement change. Otherwise its root cause decides what it is (`shared/phases.md`): suggest `/gsdd debug <the problem>`, and leave the documents as they are.
- **Work that changes no behavior**, such as refactoring, upgrades, or cleanup: no capability. Project rules it settles go to steering; when it changes what a capability's design.md states, it is a design-only change of that capability. Otherwise it needs no spec and no gsdd subcommand: say so, and leave the work to the user.
- **Exploration** whose direction is not yet clear: suggest a throwaway experiment first, and create capabilities once it is clear what to build.

A request can mix several of these; handle each part.

## 3. Settle the Intent

- Ask only what the code and the documents cannot answer. Ask the most important questions first, at most four at a time, and ask the next ones after the answers.
- When the requirements depend on knowledge that neither the user, the documents, nor the code provides, such as a standard, a regulation, the rules of the domain, or how comparable products behave, research it in primary sources, using subagents for broad searches. Use the findings to shape the questions and options you put to the user; the user decides what the requirements are. Technology is researched in the design.
- When the user leaves a point to you, or asks you to go ahead while points are still open, decide them yourself, and list these decisions in the summary for the user to check.
- Requirements say what the system does as its users and operators observe it: behavior, business rules, errors and edge cases, and quality targets they can observe, such as response times. Technology, architecture, interfaces, and data models belong to the design; do not ask about them here. When the user states them as constraints or decisions, record them, in steering when they apply to the whole project.
- In a new project, also settle the product's purpose, its users, its scope and what it leaves out, its platforms, its key quality requirements, and the capabilities its first release needs. Technology that is still open is chosen in the design of the first capability.
- When many capabilities are involved, agree on the split and the names with the user before writing their requirements; subagents can then write them in parallel.

## 4. Write

- For each capability involved, write or edit its `requirements.md`, following `shared/conventions.md` and `requirements/ears.md`.
- A new capability gets its directory and a `spec.json` in `requirements`.
- A change to a `done` capability empties its `tasks.md` and moves its phase back (`shared/phases.md`); a request that arrives during a change joins that change.
- Write the confirmed decisions that apply to the whole project into steering now, following `shared/steering.md`.
- Record the investigations that shaped the requirements, such as a standard or the behavior of comparable products, in `research.md` (`shared/documents.md`).
- When `docs/steering/settings.json` is missing, create it as `{"language": "zh"}`.

## 5. Check

Before presenting the requirements, check that:

- Every acceptance criterion can be checked by a test or an observation, and states one behavior.
- They contain no implementation detail: a criterion that cannot be written without naming a technology belongs to the design.
- Nothing is guessed: each requirement comes from the user, the documents, or the code, and what is still open is asked, not assumed.
- Vague words such as fast, reliable, or secure are made concrete, or asked about.
- The main flows, the errors, and the edge cases that users or operators can observe are covered.

## 6. Confirm

- **After writing**: present a summary of what was written, including the decisions you made for the user, and recommend whether a design is needed, by the rule in `shared/phases.md`. Then ask the user to confirm, unless they have said to proceed without asking; in that case, apply the confirmation now.
- **When the user confirms** requirements, including requirements written before this conversation, apply the confirmation at once: move the phase to `design` or `tasks`, as the user decided, or as you recommended when they left it to you, and update `updated_at`. Do not reopen the scope or ask again. The only exception is a real error, such as a criterion that cannot be checked: point it out, and ask the user before applying the confirmation.
- After applying a confirmation, suggest the next command, `/gsdd design <capability>` or `/gsdd tasks <capability>`. The next phase starts when the user asks for it.
