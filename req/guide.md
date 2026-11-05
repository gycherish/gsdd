# req

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`, `shared/capabilities.md`, `shared/steering.md`, `req/ears.md`.

The request is the arguments, or what the user has said in this conversation. With no request, ask what the user wants; when a capability is in the `req` phase, offer to continue it. When the user confirms requirements that are already written, go straight to Confirm.

This subcommand writes documents only; it never changes code.

## 1. Understand the Situation

- List the capabilities and their phases (every `spec.json` under `docs/specs/`), and read the steering.
- Read the material the user points to, such as a requirements document.
- Read the requirements of the capabilities the request may touch, and look at the code as far as needed to know what exists.
- A project without an architecture (`docs/steering/architecture.md`) is new: its requirements analysis settles its first release. gsdd starts from an empty project: when a project without capabilities already has product code, tell the user that starting from existing code is not supported yet, and stop.

## 2. Decide What the Request Is

- **A change to existing capabilities**: it changes their requirements, or only their design. A change of only the design starts at `design` (`shared/phases.md`): write nothing, and suggest `/gsdd design <capability>`. When unsure whether a request changes an existing capability or needs a new one, ask.
- **New capabilities**: new parts of the system, by the rules in `shared/capabilities.md`.
- **A bug**: behavior that breaks what the requirements say. When the requirements plainly do not cover the case, it is a requirement change. Otherwise its root cause decides what it is (`shared/phases.md`): suggest `/gsdd debug <the problem>`, and leave the documents as they are.
- **Work that changes no behavior**, such as refactoring, upgrades, or cleanup: no capability, unless it leaves an outcome that can be verified, which is a technical capability (`shared/capabilities.md`). Project rules it settles go to steering. When it changes the architecture, such as the processes or the modules, explain it, and with the user's agreement move the capabilities it affects to `arch` (`shared/phases.md`), and suggest `/gsdd arch`. When it changes what a capability's design.md states, it is a design-only change of that capability. Otherwise it needs no spec and no gsdd subcommand: say so, and leave the work to the user.
- **Exploration** whose direction is not yet clear: suggest a throwaway experiment first, and create capabilities once it is clear what to build.

A request can mix several of these; handle each part.

## 3. Settle the Intent

- Ask only what the code and the documents cannot answer. Ask the most important questions first, at most four at a time, and ask the next ones after the answers.
- When the requirements depend on knowledge that neither the user, the documents, nor the code provides, such as a standard, a regulation, the rules of the domain, or how comparable products behave, research it in primary sources, using subagents for broad searches. Use the findings to shape the questions and options you put to the user; the user decides what the requirements are. Technology is researched in the architecture and the design.
- When the user leaves a point to you, or asks you to go ahead while points are still open, recommend an answer to each, with your reason. Put the recommendations to the user with the split of the capabilities when there is one; otherwise list them in the summary, apart from what the user said. They hold only once the user confirms.
- Requirements say what the system does as its users and operators observe it: behavior, business rules, errors and edge cases, and quality targets they can observe, such as response times. Technology, architecture, interfaces, and data models belong to the architecture and the design; do not ask about them here, not even inside the options of another question. When the user states them as constraints or decisions, record them, in steering when they apply to the whole project.
- In a new project, settle only its first release, a minimum viable product (MVP): the product's purpose, its users, its platforms, its key quality requirements, and the capabilities the MVP needs. When you propose its scope, recommend the smallest one that still serves the product's purpose. The technology is chosen with the architecture, after the requirements are confirmed.
- A need left out of the MVP that is likely to come soon, and would change the shape of the architecture, is cheaper to build in from the start than to add later: for example another platform, or several users sharing the same data. Point out each such need, explain why it would be costly to add later, and recommend whether to bring it into the MVP; the user decides. What stays out is not written down (`shared/documents.md`).
- When the request creates capabilities, however few, agree on the split and the names with the user before writing their requirements. Present them as a numbered list in the order they are built, one item per capability: its name; what it holds; how it is accepted, in one sentence on what users or operators do and see; and its dependencies, one per line, as `<capability>: <behavior>` (`shared/conventions.md`). A capability's dependencies are what its acceptance needs from other capabilities: when it needs one that comes after it, the split must change (`shared/capabilities.md`). Subagents can then write them in parallel, given the split, the order, and the dependencies agreed.

## 4. Write

- For each capability involved, write or edit its `requirements.md`, following `shared/conventions.md` and `req/ears.md`.
- When criteria are dropped, renumbered, or moved to another capability, update the documents that cite them in the same step; the code, tests included, follows when the change is implemented (`shared/conventions.md`).
- A new capability gets its directory and a `spec.json` in `req`.
- A change to the requirements empties the capability's `tasks.md` and moves its phase back to `req`, also when it joins a change already under way (`shared/phases.md`).
- Write what the user has settled about the product, such as its purpose, its users, its platforms, and its scope, into `product.md`, and the other confirmed decisions that apply to the whole project into the steering, following `shared/steering.md`.
- Record the investigations that shaped the requirements, such as a standard or the behavior of comparable products, in the capability's `research.md`, or in the steering's when they shaped decisions about the whole product (`shared/documents.md`).
- When `docs/steering/settings.json` is missing, create it with the defaults (`shared/documents.md`).

## 5. Check

Before presenting the requirements, check that:

- Every acceptance criterion can be checked by a test or an observation, and states one behavior.
- They contain no implementation detail: a criterion that cannot be written without naming a technology belongs to the design.
- Nothing is guessed: each requirement comes from the user, the documents, or the code, and what is still open is asked, not assumed.
- Vague words such as fast, reliable, or secure are made concrete, or asked about.
- The main flows, the errors, and the edge cases that users or operators can observe are covered.
- The documents written describe only what is being built: nothing left out or left for later (`shared/documents.md`).
- The numbers have no gaps, and every citation in the documents names a current criterion by its current number: search them for each number that changed.
- Each capability depends on exactly the capabilities whose behavior its criteria need, and each dependency names behavior that the other capability's requirements hold (`shared/conventions.md`). Write all the capabilities out in the order they are built: one that cannot be placed is in a cycle, and the split must change (`shared/capabilities.md`).
- Each requirement is written in one capability, and the others refer to it (`shared/capabilities.md`).

## 6. Confirm

- **After writing**: present a summary of what was written, including the capabilities in the order they are built, and your recommendations on the points the user left to you, and the next phase of each capability (`shared/phases.md`): `arch` for every capability of a new project, and for a capability that does not fit the architecture, saying what it needs that the architecture does not have; otherwise `design`. Then ask the user to confirm. The requirements need their confirmation even when they have said to go ahead without asking: they decide what is built.
- **When the user confirms** requirements, including requirements written before this conversation, apply the confirmation at once: move the phase to `arch` or `design`, and update `updated_at`. Do not reopen the scope or ask again. The only exception is a real error, such as a criterion that cannot be checked: point it out, and ask the user before applying the confirmation.
- After applying a confirmation, suggest the next command: `/gsdd arch`, or `/gsdd design <capability>`. The next phase starts when the user asks for it.
