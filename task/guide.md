# task

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`.

The capability is the one named in the arguments, or the one under discussion. When several capabilities are in `task` and none is named, ask which.

This subcommand writes only `tasks.md` and `spec.json`; it never changes code.

## 1. Check the Phase

List the phase of every capability (every `spec.json` under `docs/specs/`), then check, in this order:

- Any capability in `arch`, whichever it is: the architecture is being settled, and no capability can be broken into tasks (`shared/phases.md`). Suggest `/gsdd arch`, and stop.
- A capability this one depends on that is not yet in `verify` or `done`: its work is not built yet (`shared/phases.md`). Suggest finishing it first, with the subcommand of its phase, and stop.
- This capability in `task`: continue.
- `req` or `design`: that phase must be confirmed first. Suggest `/gsdd req <capability>` or `/gsdd design <capability>`, and stop.
- `impl`: the tasks are written; suggest `/gsdd impl <capability>`. When the user asks for the tasks to be changed, edit them in place, and leave the phase as it is.
- `verify`: the tasks are done. Suggest `/gsdd verify <capability>`, and stop.
- `done`: new work on it is a change, which starts with its requirements or its design (`shared/phases.md`). Suggest describing it with `/gsdd <request>`, and stop.

## 2. Find the Gap

- Read the capability's requirements; its design.md and research.md when they exist; the steering; and tasks.md when it holds tasks of the current change.
- Study the code: for each acceptance criterion, whether the code meets it and which tests cite it (`shared/conventions.md`); which parts of the design the code does not have yet; and what in the code, such as its tests and scripts, still follows the requirements or the design as they were before the change (git shows how they changed).

## 3. Break Down

- Cover the gap and nothing else: each acceptance criterion that the code does not meet or that no test cites, each part of the design that the code does not have, and whatever in the code still follows them as they were before the change. Work that the requirements and the design do not ask for gets no task.
- Order the tasks so that each needs only the existing code and the tasks before it. The design's unverified assumptions are checked first, before the tasks that rely on them. Setup comes before the tasks that use it, such as a project skeleton that follows the code organization of the architecture and builds and runs its tests, a new dependency, or a test harness the design calls for.
- Make each task one piece that can be implemented, tested, and reviewed on its own within one conversation, that leaves the project building with all its tests passing, and whose outcome can be observed.
- List on each task the acceptance criteria it makes true. Their tests are written in that task, so no task is only for writing tests. A criterion that cannot be tested automatically gets a manual check task: what to check, how, and what result passes.
- Title each task with what will be true when it is done. Add only what an implementer would otherwise have to work out: the section of the design it implements, referred to rather than restated; the parts it touches; or a dependency that the order does not show.
- When tasks.md already holds tasks of the current change, edit it in place: keep the checked tasks that still hold, and change or add the others.
- When the tasks would need a decision that the requirements, the architecture, or the design should make but do not, or when they contradict each other, explain it to the user; once they agree, move the phase back to `req`, `arch`, or `design`, and suggest that subcommand. Do not make that decision in the tasks.

## 4. Check

Before writing, check that:

- Each criterion in the gap is listed on a task, and each criterion left out is met by the code and cited by a test that verifies it.
- Each part of the design in the gap is covered by a task.
- No task needs the work of a later task.
- No task is too large for one conversation, or too small to show an outcome of its own.

## 5. Write

- Write tasks.md, following `shared/conventions.md`; move the phase to `impl`, and update `updated_at`. When there is no gap, say so in tasks.md, and move the phase to `verify` instead.
- Present a short summary: the tasks in order, and what the user should know before implementation starts: work beyond the change itself, such as acceptance criteria thought done that the code does not meet; what needs the user, such as manual checks on real devices; and the checks whose result may send the capability back to the design. Then suggest `/gsdd impl <capability>`, or `/gsdd verify <capability>` when there was no gap.
- The tasks need no confirmation; change them when the user asks. Implementation starts when the user asks for it.
