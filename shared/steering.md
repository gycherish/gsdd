# Writing Steering

These rules apply to every subcommand that writes `docs/steering/`.

## Content

- Write what a reader needs before reading the code: the choices, patterns, and conventions the code follows throughout, with their reasons when known. Leave out what the code already says, such as directory trees, what each file or module does, dependency lists, defaults, formats, and constants: new code that follows the existing patterns should not require a steering update. For example, instead of a tree of `server/` with a line per file, write "each package under `server/` owns one resource (sessions, files, users) and exposes it only through its `api` module".
- Write only what the code, the project's documents, or the user's decisions establish. Anything else that matters is an open question: write it as one, never as a guess or a conclusion.
- Write only what applies to the whole project. A decision that concerns one capability goes to that capability's design.md.
- Describe the product's scope in a few sentences, without listing the capabilities: the directories under `docs/specs/` are that list.
- Never write secrets, such as keys, passwords, credentials, or internal addresses.
- Do not describe gsdd itself, or agent tool directories such as `.claude/`.

## Files

- Create a file when there is something to write in it, and choose its sections to fit its content. Keep each file short enough to read in full.
- A topic that outgrows the core files gets its own file, named in lowercase English words joined by hyphens (`testing.md`).
- When `settings.json` is missing, create it as `{"language": "zh"}`.

## Changes

- Edit in place.
- Facts are what the code uses and how to work with it, such as commands, versions, or paths. Correct them without asking when the code has changed.
- Decisions are statements that carry a reason, and rules such as conventions or constraints. Changing or removing one needs the user's agreement, even when the code no longer follows it. A decision the user has just confirmed counts as agreement; say what it replaces.
- Tell the user which files changed and why.
