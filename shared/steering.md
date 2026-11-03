# Writing Steering

These rules apply to every subcommand that writes `docs/steering/`.

## Content

- Write what a reader needs before reading the code: the choices, patterns, and conventions that hold throughout the code, with their reasons. Leave out what the code already says, such as directory trees, what each file or module does, dependency lists, defaults, formats, and constants: new code that follows the existing patterns should not require a steering update. For example, instead of a tree of `server/` with a line per file, write "each package under `server/` owns one resource (sessions, files, users) and exposes it only through its `api` module".
- Write only what the code, the project's documents, or the user's decisions establish. What is still open is not written: tell the user instead, and never write a guess as a conclusion.
- Write only what applies to the whole project. A decision that concerns one capability goes to that capability's design.md, except the choice of a technology, such as a third-party library, which goes to `tech.md` even when one capability uses it.
- State each decision with its reason, and leave the evidence, such as compared alternatives and experiments, to `research.md` (`shared/documents.md`).
- Describe the product's scope in a few sentences, without listing the capabilities: the directories under `docs/specs/` are that list.
- Never write secrets, such as keys, passwords, credentials, or internal addresses.
- Do not describe gsdd itself, or agent tool directories such as `.claude/`.

## Decisions for the Whole Project

- The technology is the user's to choose: the language and its standard, the build system, the package management, the third-party libraries, and the code conventions, such as naming and error handling. Write it only once the user has chosen it, even when the user has said to go ahead without asking.
- So are the other decisions that bind the whole project and that the code does not already establish, such as the product's users and platforms: they hold only once the user confirms them, with the requirements or the architecture.
- Before putting the technology to the user, research each item in primary sources, using subagents for broad searches: the options in current use, what established comparable products or open-source projects use and why, and each option's maturity, maintenance, license, platform support, and known issues. When the research cannot be done, such as when web access is not allowed, say so before the options, mark what comes only from memory, and ask whether the user can allow it.
- Put the technology to the user as one list. For each item: the options; what the research found, such as the current best practice and what comparable products use, each finding with its source; the reasons for and against each option; and your recommendation, with the reason when it departs from current practice. The list covers each item the project needs, even one with an obvious answer, such as the version of the language. Record the research in `research.md` (`shared/documents.md`).
- An answer such as "go with your recommendations" covers only what the list held. A decision of this kind that comes up later, such as while designing, is put to the user the same way before it is written.

## Files

- Create a file when there is something to write in it, and choose its sections to fit its content. Keep each file short enough to read in full.
- A topic that outgrows the core files may get its own file once the user agrees, named in lowercase English words joined by hyphens (`testing.md`).

## Changes

- Edit in place.
- What the steering says was decided or confirmed by the user, or follows from what they decided, such as the commands of the build system they chose. Changing or removing it needs the user's agreement, even when the code no longer follows it. A decision the user has just confirmed counts as agreement; say what it replaces.
- Tell the user which files changed and why.
