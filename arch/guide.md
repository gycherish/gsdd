# arch

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/capabilities.md`, `shared/steering.md`, `arch/architecture.md`.

This subcommand chooses the technology of a new project and designs the architecture of the whole system, and changes the architecture when capabilities need it.

This subcommand writes `architecture.md`, `tech.md`, the steering's `research.md`, and `spec.json`; it never changes code.

## 1. Check the Phase

A run works for the capabilities in `arch`, such as every capability of a new project's first release once its requirements are confirmed. The arguments name a capability only when a later phase has found a problem in the architecture.

- No architecture yet (`docs/steering/architecture.md`), and a capability still in `req`: the first architecture waits until the requirements of the whole first release are confirmed. Suggest `/gsdd req <capability>`, and stop.
- A capability named in the arguments:
  - In `design`, `task`, `impl`, `verify`, or `done`: the problem was found there (`shared/phases.md`). Move it to `arch`, and update `updated_at`; a `done` one starts a change, and its `tasks.md` is emptied.
  - In `req`: its requirements must be confirmed first. Suggest `/gsdd req <capability>`, and stop.
- No capability in `arch`: the architecture changes only when a capability needs it. Say so, suggest describing the change with `/gsdd <request>`, and stop.

## 2. Understand the Situation

- Read the steering, and the requirements of every capability; those in `arch` are the reason for this run.
- When the architecture exists, also read the designs of the capabilities the change may affect, and the code as far as needed.

## 3. Design

- **For the first architecture, technology first.** Put to the user the technology the whole project will use (`shared/steering.md`); a choice the user has already made is recorded, not asked again. With the build system, settle how the project is built, tested, and run. When a fact a choice depends on is uncertain, such as whether a library supports the target platform, check it in primary sources or with a small throwaway experiment outside the project.
- **Then the architecture**, by `arch/architecture.md`, from the whole system down to its modules, covering every capability. Ask the user about the choices that are theirs, such as how the product is deployed, and recommend an answer.
- The architecture serves the requirements that are written. When a need they leave out is likely to come soon and would give the architecture another shape, point it out, and recommend whether to add it to the requirements first; the user decides. When they add it, suggest `/gsdd req`, and stop: the architecture waits for its requirements.
- **A change** to a confirmed architecture changes only what the capabilities in `arch` need, and keeps the rest. Technology it needs that `tech.md` does not have is put to the user as for the first architecture. Before writing the change, explain it to the user: what changes, why, and the capabilities whose designs or requirements it affects. Write it once the user agrees, so that capabilities still in `impl` or `verify` never read a change the user has not agreed to.

## 4. Write

- Write `architecture.md`, and the technology into `tech.md`, following `shared/steering.md`.
- Record the evidence behind these decisions, such as the alternatives compared and the experiments run, in the steering's `research.md` (`shared/documents.md`).

## 5. Review

- Have a subagent with a fresh context review the architecture by `arch/architecture.md`, unless the change leaves the processes, the modules, and their dependencies as they are. Give it the gsdd directory and the language (`SKILL.md`), the path of that file, and the paths of the documents and the code, not your conclusions. Fix what it finds that holds, and tell the user what you did not fix and why.

## 6. Confirm

- Present a summary: for the first architecture, the technology the user chose; the processes and modules, and where each capability lands; the main decisions, including those you made; for a change, what changed and the capabilities it affects; and the risks and unverified assumptions. Ask the user to confirm the architecture; it needs their confirmation even when they have said to go ahead without asking.
- When the user confirms, move the capabilities in `arch` to `design`, and update `updated_at`. Move each other capability whose design no longer fits the architecture back to `design`, and each whose requirements must change back to `req`, as the user decides; a `done` one starts a change, and its `tasks.md` is emptied (`shared/phases.md`).
- Suggest designing the capabilities in the order of their dependencies, starting with `/gsdd design <capability>`.
