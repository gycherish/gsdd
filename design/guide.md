# design

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`, `shared/capabilities.md`, `design/principles.md`. Before writing steering, also read `shared/steering.md`.

The capability is the one named in the arguments, or the one under discussion. When several capabilities are in `design` and none is named, ask which. When the user confirms a design that is already written, go straight to Confirm.

This subcommand writes documents and throwaway experiments; it never changes the product code.

## 1. Check the Phase

List the phase of every capability (every `spec.json` under `docs/specs/`), then check, in this order:

- Any capability in `arch`, whichever it is: the architecture is being settled, and no capability can be designed (`shared/phases.md`). Suggest `/gsdd arch`, and stop.
- This capability in `design`: continue.
- `done`: when the user wants its design changed, this is a design-only change: empty its `tasks.md`, and move its phase to `design` (`shared/phases.md`). Otherwise, say that it is done, and stop.
- `req`: its requirements must be confirmed first. Suggest `/gsdd req <capability>`, and stop.
- `task`, `impl`, or `verify`: a design problem found there moves the phase back to `design` (`shared/phases.md`).

## 2. Understand the Situation

- Read the capability's requirements; its design.md and research.md when they exist, which are edited in place; the steering, and in it the architecture, which places the capability in the modules it lands in; and the requirements and designs of the capabilities it depends on.
- Study the code the change touches: what already serves each requirement, what is missing, and what constrains the approach.
- When the capability relies on an interface owned by a capability it depends on, and that one needs a design but has none yet, suggest `/gsdd design <that capability>` first, and stop: the interface is its owner's to define (`shared/capabilities.md`).

## 3. Investigate

Investigate as far as the design needs, using subagents for broad searches.

- For technology the design introduces, read primary sources, such as official documentation, specifications, and source repositories: versions, licensing, limits, and known issues.
- When a technical fact the design depends on is uncertain, check it with a small throwaway experiment, outside the product code, before relying on it. When that is not possible now, list it as an unverified assumption, to be checked first.
- Record what supports the decisions in the capability's `research.md` (`shared/documents.md`).

## 4. Decide

- For each choice with real alternatives, compare them and choose one, following `design/principles.md`.
- Ask the user only about the decisions that are theirs to make, such as product trade-offs, costs, or preferences, and recommend an answer. When the answer would change the rest of the design, ask before writing it. Decide the rest yourself, and list those decisions in the summary.
- The design works within the architecture: the processes, the modules and their dependencies, and the direction of the mechanisms every module follows are the architecture's; the details within them are the design's. When the capability needs one the architecture does not have, or does not fit it, explain it to the user; once they agree, move the phase to `arch`, suggest `/gsdd arch`, and stop.
- A change to the technology, such as a third-party library that `tech.md` does not name or a different build command, is the user's choice (`shared/steering.md`): put the options to them before the design relies on it. It is not a change of the architecture, unless it changes the processes, the modules, or their dependencies.
- A requirement problem found here, such as a contradiction, a gap, or parts that should be separate capabilities, goes to the user: explain it, and move the phase back to `req` once they agree.

## 5. Write

- Write `design.md` as `design/principles.md` describes.
- Write the technology the user has chosen into `tech.md`, following `shared/steering.md`. Before writing it, check each decision in it against what the user answered, and put each one they have not chosen to them first. The architecture changes only through `/gsdd arch`.

## 6. Review

- Check the draft against `design/review.md`.
- Unless the design is small, also have a subagent with a fresh context review it by `design/review.md`. Give it the gsdd directory and the language (`SKILL.md`), the path of `design/review.md`, and the paths of the documents and the code it needs, not your conclusions. Fix what it finds that holds, and tell the user what you did not fix and why.

## 7. Confirm

- **After writing**: present a summary: the main decisions, including those you made for the user; the risks and unverified assumptions; and any open questions. Ask the user to confirm, unless they have said to proceed without asking; in that case, apply the confirmation now.
- **When the user confirms** a design, including one written before this conversation, apply the confirmation at once: move the phase to `task`, and update `updated_at`. Do not reopen the design or ask again. The only exception is a real error, such as a requirement the design does not serve or a risk it ignores: point it out, and ask the user before applying the confirmation.
- After applying a confirmation, suggest `/gsdd task <capability>`. The next phase starts when the user asks for it.
