# review

Read first: `shared/principles.md`, `shared/documents.md`, `shared/conventions.md`. When a problem concerns the steering, also read `shared/steering.md`.

The change is what the arguments name, such as uncommitted work, a commit or a range of commits, a branch, a pull request, or a task of a capability. With no arguments, it is the uncommitted work; when there is none, ask what to review.

This subcommand only reports; it changes neither the code nor the documents. Experiments that need to change files, such as breaking a behavior to see whether its test fails, run on a copy outside the project.

## 1. Choose Who Reviews

- If you wrote or edited any part of the change earlier in this conversation, do not review it yourself: your own reasoning would steer the review. Have a subagent with a fresh context review it by this guide: give it the path of this guide, the change, and the paths of the documents, not your conclusions. Pass on everything it reports; when you think a problem does not hold, say why instead of leaving it out.
- Otherwise, review it yourself.

## 2. Understand the Change

- Read the diff, and the changed files in full where the diff is not enough.
- Find what the change is for: the task it implements, or the capabilities whose requirements or design describe the code it touches. Read their requirements, design, and tasks, and the steering.

## 3. Check

- **Criteria**: each acceptance criterion the change serves is met, and a test that cites it (`shared/conventions.md`) would fail if the behavior broke. A criterion that cannot be tested automatically needs a manual check task instead.
- **Design**: the code follows the design's structure, contracts, and decisions. A departure is a problem even when the code works.
- **Steering**: the code follows the conventions and constraints the steering sets.
- **Scope**: the change adds no behavior that the requirements and the design do not ask for, and the change for a task does not do the work of other tasks.
- **Correctness**: the changed code has no bugs, unhandled failures, or security problems, and leaves no stub or placeholder where real behavior is required.
- **Documents**: no statement in the requirements, the design, or the steering is untrue after the change.
- **Evidence**: run the tests and the checks the steering names, and note the commands and their results.

## 4. Report

- Confirm each problem in the code, or by running it, before reporting it; mark the ones you could not confirm.
- Give the verdict first: passes, or needs changes.
- List every problem that must be fixed, the most important first. For each: what is wrong, where (file and line), what it breaks (the criterion, the design section, or the convention), and what to change. When the code and a document disagree, say which should change; changing a decision in a document needs the user's agreement.
- Add at most three suggestions, only when they are worth the change, and nothing about style that the steering does not set.
- Give the commands run and their results. Suggest the next step, such as `/gsdd debug <the problem>` for a failure whose cause is unclear, or `/gsdd design <capability>` when the design itself is the problem.
