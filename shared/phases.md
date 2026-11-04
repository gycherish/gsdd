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
- `phase` is one of `req`, `arch`, `design`, `task`, `impl`, `verify`, and `done`. Each phase but `done` is worked on by the subcommand of the same name.

## Transitions

```
req →(user confirms)→ arch →(user confirms)→ design →(user confirms)→ task → impl → verify →(user accepts)→ done
```

- A new capability starts in `req`.
- When the user confirms the requirements, the next phase is:
  - `arch` for every capability while the project has no architecture yet (`docs/steering/architecture.md`), and for a capability that does not fit the architecture: one that needs a process, a module, a platform, or a mechanism that the architecture does not have.
  - Otherwise `design`. Every capability is designed, so that every change takes the same path: a small change gets a short design, and a design the change does not affect stays as it is.
- The architecture, once confirmed, changes only when a capability needs it, and only as far as it needs. A change to the technology, such as a third-party library or a build command, is not a change of the architecture unless it changes the processes, the modules, or their dependencies: it is made in the design of the capability that needs it. While any capability is in `arch`, no capability is designed or broken into tasks, since work built on the architecture now could be lost; capabilities in `impl` and `verify` go on.
- When the user confirms the architecture, the capabilities in `arch` move to `design`. Those whose design no longer fits it go back to `design`, and those whose requirements must change go back to `req`, as the user decides (`arch/guide.md`).
- A capability is broken into tasks only once every capability it depends on has done all its tasks, and is in `verify` or `done`: its tasks build on their code.
- Tasks need no confirmation: writing tasks.md moves the phase to `impl`.
- When every task is checked, the phase moves to `verify`. If verification fails, it goes back to `impl` with the problems found.
- A problem found in a later phase sends the capability back: a requirement problem to `req`; a problem in the architecture, such as a module boundary or a mechanism every module follows, to `arch`; a design problem or a problem in the technology, to `design`.
- `done` requires every task in tasks.md to be checked and the verification results to be recorded in it.

## Changes

- A request that changes the requirements, the architecture, or the design of a `done` capability starts a new change: tasks.md is emptied, and the phase goes back to `req`, `arch`, or `design`, the earliest one the request affects.
- A capability has one change at a time: a request that arrives during a change joins it, and the phase goes back to the earliest phase the request affects. When the request changes the requirements, tasks.md is emptied too: the tasks are planned again from the new requirements.
- Work that changes neither the requirements, the architecture, nor the design is not a change, and the phase stays as it is.
- A bug is classified by its root cause. When the code does not follow the design, the fix restores an existing acceptance criterion and is not a change: it needs only the fix and a test that cites the criterion. When the design cannot meet the criterion, it is a design-only change; when the requirements do not cover the case, it is a requirement change. A quick mitigation may go in first, but the capability stays in the change until the full fix is accepted.

## Progress and Confirmation

- Progress within a phase, such as whether the requirements are written and waiting for confirmation, is not recorded: work it out from the documents and git, and ask when unsure.
- Only the user confirms and accepts. Present what was written and wait for the answer; never proceed past a confirmation on the user's behalf unless the user has said to proceed without asking. The requirements and the architecture always wait for the user's confirmation: mistakes in them cost the most.
