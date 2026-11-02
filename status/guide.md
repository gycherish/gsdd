# status

Read first: `shared/documents.md`, `shared/phases.md`, `shared/conventions.md`.

The capabilities are the one named in the arguments, or else all of them.

This subcommand only reports; it changes nothing.

## 1. Look

- List the capabilities and their phases (every `spec.json` under `docs/specs/`), and the capabilities their requirements depend on.
- Work out where each one stands within its phase from its documents and git (`shared/phases.md`), and say so when it cannot be told: requirements or a design written and waiting for confirmation, or not written yet; in `implementation`, the tasks checked and the next one; in `verification`, waiting to be verified and accepted.
- Check the working tree: its uncommitted changes, and the task they belong to, such as a task done and waiting for the user's review and commit.
- Note what does not fit, such as a phase of `implementation` with every task checked, or a capability designed before the capabilities it depends on.
- When there are no capabilities: a project with no code is new; suggest describing it with `/gsdd <what to build>`. A project with code but no steering: suggest `/gsdd steering`, and then `/gsdd <request>`.

## 2. Report

- One line per capability: its phase, where it stands, what it waits for, and the command to run next.
- Then recommend what to do now, such as a confirmation or a commit that other work waits on, or the capability that others depend on.
- For a capability named in the arguments, also list its unchecked tasks and the capabilities that depend on it.
