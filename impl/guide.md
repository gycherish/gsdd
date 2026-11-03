# impl

Read first: `shared/principles.md`, `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`.

The capability is the one named in the arguments, or the one under discussion. When several capabilities are in `impl` and none is named, ask which.

## Settings

`impl` in `docs/steering/settings.json` (`shared/documents.md`) sets:

- `auto_commit`, default `false`: when on, each task is committed once it is done; when off, the user reviews and commits it.
- `tasks_per_run`, default `1`: the most tasks one run does when `auto_commit` is on. When `auto_commit` is off, a run does one task, so that each task stays one commit.

A run takes the tasks named in the arguments, or else the unchecked tasks, in the order of tasks.md, and does as many as these settings allow; it stops earlier when something needs the user. When a named task comes after unchecked ones, say that it may need them, and ask before starting.

## 1. Check the Phase

- `impl`: continue.
- `req`, `arch`, `design`, or `task`: the tasks are not ready. Suggest the subcommand of that phase, and stop.
- `verify`: every task is done. Suggest `/gsdd verify <capability>`, and stop.
- `done`: new work on it is a change, which starts with its requirements or its design (`shared/phases.md`). Suggest describing it with `/gsdd <request>`, and stop.

## 2. Prepare

- When the working tree has uncommitted changes, such as an earlier task not yet committed, tell the user and ask before starting: they would be mixed with the task's changes.
- Read the capability's requirements, its design.md and research.md when they exist, the steering, and tasks.md.

## 3. Do Each Task

Do the tasks one at a time, in order, each through steps 3 to 5.

- Study the code the task touches. When the task needs a decision that the requirements, the design, or the architecture should make but do not, when the documents contradict each other, or when the task needs work that no task covers, explain it to the user, suggest `/gsdd req`, `/gsdd design`, or `/gsdd task` for the capability, or `/gsdd arch <capability>` for the architecture, and stop. Do not make that decision yourself.
- Write the tests first: for each acceptance criterion the task lists, a test that cites it (`shared/conventions.md`). Run them, and see each fail for the reason expected, before writing the code. A task that lists no criteria, such as setting up the project, shows its outcome another way, such as a command that now works.
- Write what the task and the design call for, and nothing else: no work of other tasks, no behavior the documents do not ask for, and no technology that `tech.md` does not name.
- Run all the tests and the checks the steering names; all must pass.
- A task that checks an assumption records the experiment and its result in research.md (`shared/documents.md`). When the assumption does not hold, the design must change: explain it to the user, suggest `/gsdd design <capability>`, and stop.
- A manual check: do it yourself when you can, such as by running the program and reading its output; otherwise give the user its steps, and stop until they report the result. Record under the task who checked, how, and the result. A check that fails is a failure.
- When something fails and the cause is not clear, or a fix did not work, follow `debug/guide.md`.
- This subcommand does not write the steering. When the work shows that the steering is wrong, such as a command in `tech.md` that does not work, explain it to the user, suggest `/gsdd design <capability>`, and stop.

## 4. Review

- When the task changed code, have it reviewed by `review/guide.md`; the change is the task, in the files changed for it.
- Fix each problem that must be fixed, run the tests again, and have the task reviewed again. When the second review still finds a problem that must be fixed, show it to the user, and stop.
- Do not apply the review's suggestions unasked; list them for the user.

## 5. Finish

- Check the task (`- [x]`) once it is done: its tests pass, its review has passed, and a manual check has its result recorded. Update `updated_at` (`shared/phases.md`); when every task is checked, move the phase to `verify`.
- With `auto_commit` on, commit the task: only the files changed for it, the documents included, with a message that follows the project's conventions and names the capability and the task. Never push.
- Go on to the next task while the run allows another.

## 6. Report

- For each task done: what changed; the tests that failed before the code and pass after it; the review's result, with the suggestions not applied; and the commit, when one was made.
- What needs the user: a manual check waiting for its result, the problem that stopped the run, and, with `auto_commit` off, the task waiting for their review and commit.
- The next step: `/gsdd impl <capability>` for the next task, or `/gsdd verify <capability>` when every task is checked.
