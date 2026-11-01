# Phases

Each capability's `spec.json` holds exactly these fields:

```json
{
  "created_at": "2026-11-01T10:00:00+08:00",
  "updated_at": "2026-11-03T15:20:00+08:00",
  "phase": "design"
}
```

- `created_at` is when the capability's directory was created, and `updated_at` is the last time gsdd changed any file in it. Write ISO 8601 with the UTC offset, read from the system clock (for example, by running a command); never estimate.
- `phase` is one of `requirements`, `design`, `tasks`, `implementation`, `verification`, and `done`. The subcommands for these phases are `requirements`, `design`, `tasks`, `impl`, and `verify`.

## Transitions

```
requirements →(user confirms)→ design →(user confirms)→ tasks → implementation → verification →(user accepts)→ done
```

- A new capability starts in `requirements`.
- When the user confirms the requirements, decide with them whether a design is needed; if not, go straight to `tasks`. A design is needed when the change adds or alters structure, interfaces between components or processes, persisted data formats, concurrency, or security; changes something design.md states; or rests on assumptions not yet verified.
- Tasks need no confirmation: writing tasks.md moves the phase to `implementation`.
- When every task is checked, the phase moves to `verification`. If verification fails, it goes back to `implementation` with the problems found.
- A problem found in a later phase sends the capability back: a requirement problem to `requirements`; a design problem, or a finding that a design is needed after all, to `design`.
- `done` requires every task in tasks.md to be checked and the verification results to be recorded in it.

## Changes

- A request that changes the requirements or the design of a `done` capability starts a new change: tasks.md is emptied, and the phase goes back to `requirements`, or to `design` when the requirements stay as they are.
- A capability has one change at a time: a request that arrives during a change joins it, and the phase goes back to the earliest phase the request affects.
- Work that changes neither the requirements nor the design is not a change, and the phase stays as it is. A bug fix that restores an existing acceptance criterion needs only the fix and a test that cites the criterion.

## Progress and Confirmation

- Progress within a phase, such as whether the requirements are written and waiting for confirmation, is not recorded: work it out from the documents and git, and ask when unsure.
- Only the user confirms and accepts. Present what was written and wait for the answer; never proceed past a confirmation on the user's behalf unless the user has said to proceed without asking.
