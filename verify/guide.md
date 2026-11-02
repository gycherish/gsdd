# verify

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`.

The capabilities are the ones named in the arguments; with no arguments, those in `verification`. When none is, ask which capability to check.

This subcommand changes no code, and writes only tasks.md and spec.json.

## 1. Check the Phase

For each capability:

- `verification`: verify the change, and have the user accept it (steps 2 to 4).
- `done`: check that its documents and the code still agree (steps 2, 3, and 5).
- `implementation`: tasks remain. Suggest `/gsdd impl <capability>`, and stop.
- `requirements`, `design`, or `tasks`: suggest the subcommand of that phase, and stop.

## 2. Choose Who Checks

- If you wrote or edited any of the code being checked earlier in this conversation, do not check it yourself: your own reasoning would steer the checks. Have a subagent with a fresh context do only step 3 of this guide and report what it finds, with its evidence: give it the gsdd directory and the language (`SKILL.md`), the path of this guide, and the capability, not your conclusions. Pass on everything it reports; when you think a finding does not hold, say why.
- Otherwise, check it yourself. Subagents may check several capabilities in parallel.

## 3. Check

Read the capability's requirements, its design.md and research.md when they exist, tasks.md, and the steering. Experiments that need to change files run outside the project.

- **Evidence**: run all the tests and the checks the steering names, and note the commands and their results. Where the program can run here, use it as its users would, such as by starting it and trying the behavior the capability provides.
- **Criteria**: each acceptance criterion that is not deleted is met by the code, and is cited by a test that passes (`shared/conventions.md`) or has a recorded manual check. No test cites a deleted criterion.
- **Design**: the code follows the design as a whole, in its structure, the interfaces between its parts, its data, and its decisions, and the parts work together end to end.
- **Documents**: no statement in the requirements, the design, research.md, or the steering is untrue, and each assumption the design rests on has been checked.
- **Tasks**, for a change: every task is checked, and every manual check has its result recorded.

Confirm each problem in the code, or by running it, before reporting it, and find where its cause lies: the code, the design, or the requirements; say so when it is not clear yet.

## 4. Accept the Change

- When the checks find problems, show them first: for each, what is wrong, where, and what it breaks.
  - For each problem in the code, or whose cause is not clear yet, add a task to tasks.md that names it and the criteria it breaks (`shared/conventions.md`).
  - When a problem lies in the design or the requirements, explain it, and suggest `/gsdd design <capability>` or `/gsdd requirements <capability>`, which move the phase back. Otherwise, move the phase back to `implementation`, update `updated_at`, and suggest `/gsdd impl <capability>`.
- When they find none, present the verification: for each acceptance criterion, the test or the manual check that verifies it; the commands run with their results; what you tried in the program; and steps for the user to try the change themselves, with what they should see. Then ask the user to accept the change.
- When the user accepts, record the verification in tasks.md: the commands and their results, what was checked by hand, and that the user accepted. Move the phase to `done`, and update `updated_at` (`shared/phases.md`).
- When the user does not accept, their reasons are problems: handle them as above.

## 5. Report Disagreements

For a `done` capability, change no document, and leave the phase as it is. Report:

- The result: the documents and the code agree, or where they do not.
- For each disagreement, what it is, where, and the next step. A criterion that the code does not meet is a bug: suggest `/gsdd debug <the problem>`. When a document and the code differ in another way, say which of them should change, or ask the user when that cannot be told; a document changes through its own subcommand, such as `/gsdd design <capability>`. A criterion that the code meets but no test cites is left to the user to add a test for.
- The commands run and their results.
