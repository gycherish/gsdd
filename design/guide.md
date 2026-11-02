# design

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`, `shared/capabilities.md`, `design/principles.md`. Before writing steering, also read `shared/steering.md`.

The capability is the one named in the arguments, or the one under discussion. When several capabilities are in `design` and none is named, ask which. When the user confirms a design that is already written, go straight to Confirm.

This subcommand writes documents and throwaway experiments; it never changes the product code.

## 1. Check the Phase

- `design`: continue.
- `done`, and the user wants its design changed: this is a design-only change. Empty its `tasks.md`, and move its phase to `design` (`shared/phases.md`).
- `requirements`: its requirements must be confirmed first. Suggest `/gsdd requirements <capability>`, and stop.
- `tasks`, `implementation`, or `verification`: a design problem found there moves the phase back to `design` (`shared/phases.md`).

## 2. Understand the Situation

- Read the capability's requirements; its design.md and research.md when they exist, which are edited in place; the steering; and the requirements and designs of the capabilities it depends on.
- Study the code the change touches: what already serves each requirement, what is missing, and what constrains the approach.

## 3. Investigate

Investigate as far as the design needs, using subagents for broad searches.

- For technology the design introduces, read primary sources, such as official documentation, specifications, and source repositories: versions, licensing, limits, and known issues.
- When a technical fact the design depends on is uncertain, check it with a small throwaway experiment, outside the product code, before relying on it. When that is not possible now, list it as an unverified assumption, to be checked first.
- Record what supports the decisions in `research.md` (`shared/documents.md`).

## 4. Decide

- For each choice with real alternatives, compare them and choose one, following `design/principles.md`.
- Ask the user only about the decisions that are theirs to make, such as product trade-offs, costs, or preferences, and recommend an answer. When the answer would change the rest of the design, ask before writing it. Decide the rest yourself, and list those decisions in the summary.
- In a new project, the first design also chooses the technology: compare the real options, recommend one, and let the user decide before the design is written. Record the decision in steering.
- A requirement problem found here, such as a contradiction, a gap, or parts that should be separate capabilities, goes to the user: explain it, and move the phase back to `requirements` once they agree.

## 5. Write

- Write `design.md` as `design/principles.md` describes.
- Write the decisions that apply to the whole project into steering now, following `shared/steering.md`.

## 6. Review

- Check the draft against `design/review.md`.
- Unless the design is small, also have a subagent with a fresh context review it by `design/review.md`. Give it the paths of the documents and the code it needs, not your conclusions. Fix what it finds that holds, and tell the user what you did not fix and why.

## 7. Confirm

- **After writing**: present a summary: the main decisions, including those you made for the user; the risks and unverified assumptions; and any open questions. Ask the user to confirm, unless they have said to proceed without asking; in that case, apply the confirmation now.
- **When the user confirms** a design, including one written before this conversation, apply the confirmation at once: move the phase to `tasks`, and update `updated_at`. Do not reopen the design or ask again. The only exception is a real error, such as a requirement the design does not serve or a risk it ignores: point it out, and ask the user before applying the confirmation.
- After applying a confirmation, suggest `/gsdd tasks <capability>`. The next phase starts when the user asks for it.
