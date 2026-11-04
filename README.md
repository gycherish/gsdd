# gsdd

gsdd is spec-driven development for [Claude Code](https://claude.com/claude-code), packaged as a single skill. It rests on three principles:

- **Intent before code.** What you want, and the decisions behind it, are written down before the code.
- **Documents stay true.** When the code changes, the documents change with it; when the two disagree, one of them gets fixed.
- **Done means verified.** Every acceptance criterion is checked by a test or a recorded manual check, and nothing counts as done without fresh evidence.

gsdd started from [cc-sdd](https://github.com/gotalab/cc-sdd) 3.1.0.

## How It Works

Each part of your system with its own behavior is a *capability*, with its own directory of documents. A capability moves through these phases, each named after the subcommand that works on it:

```
req ─(you confirm)→ arch ─(you confirm)→ design ─(you confirm)→ task → impl → verify ─(you accept)→ done
```

- `req` settles what you want. In a new project, it settles the product (its purpose, its users, and its platforms) and a minimum viable product (MVP). A need left out that is likely to come soon and would change the architecture is pointed out, for you to decide whether it joins the MVP. The points you leave to it come back as recommendations for you to agree to, and the requirements always wait for your confirmation.
- `arch` designs the architecture of the whole system once the MVP's requirements are confirmed. It first researches the technology and puts it to you, item by item, with the current best practice and what comparable products use, and then settles the direction of the system, its processes and modules, leaving the details to each capability's design. Once you confirm it, it changes only when a new or changed requirement needs it, and the capabilities it affects are designed again. A new library or a changed build command is not a change of the architecture: it is chosen in the design of the capability that needs it. While it changes, no capability is designed or broken into tasks.
- A capability that fits the architecture skips `arch`. Every capability is designed, so that every change takes the same path: a small change gets a short design.
- The tasks cover only the gap between the documents and the code. A capability is broken into tasks once the capabilities it depends on are built.
- Implementation goes one task at a time: tests first, then an independent review.
- A problem found in a later phase sends the capability back to the phase it belongs to.
- A change to a finished capability starts again from its requirements, or from its design when the requirements stay as they are.

`review`, `debug`, and `status` can be used at any time.

## Install

Add gsdd to a project as a git submodule:

```bash
git submodule add https://github.com/gycherish/gsdd.git .claude/skills/gsdd
```

Whoever clones the project then needs `git clone --recurse-submodules`, or `git submodule update --init` after cloning. To update gsdd later:

```bash
git submodule update --remote .claude/skills/gsdd
git add .claude/skills/gsdd
git commit -m "chore: update gsdd"
```

To use gsdd in all your projects instead, clone it into your personal skills directory:

```bash
git clone https://github.com/gycherish/gsdd.git ~/.claude/skills/gsdd
```

## Use

```
/gsdd <what you want>            a new feature, a change, or a bug, in your own words
/gsdd <subcommand> [arguments]
/gsdd                            where things stand, and what to run next
```

| Subcommand | What it does |
|---|---|
| `req` | Settles the intent of a request and writes the requirements; `/gsdd <what you want>` runs it |
| `arch` | Designs the architecture of the whole system, and changes it when requirements need it |
| `design` | Designs a capability |
| `task` | Breaks the gap between the documents and the code into tasks |
| `impl` | Implements the next task: tests first, then a review |
| `verify` | Verifies a change and asks you to accept it; on a finished capability, checks that the documents and the code still agree |
| `review` | Reviews a change, such as uncommitted work or a commit, against the documents |
| `debug` | Finds the root cause of a failure, and fixes it when the cause lies in the code |
| `status` | Shows where each capability stands, and what to run next |

### Getting Started

gsdd starts from an empty project; starting from existing code is not supported yet. Describe what you want to build, or point to a requirements document, with `/gsdd <your idea>`. The first requirements analysis settles the product and the capabilities of a minimum viable product. Then run `/gsdd arch` to choose the technology and design the architecture, and design the capabilities one by one.

### A Typical Change

1. `/gsdd <request>`: answer the questions, read the requirements, and confirm them.
2. `/gsdd arch`, only when the change needs the architecture to change: read the change, and confirm it.
3. `/gsdd design <capability>`: read the design, and confirm it.
4. `/gsdd task <capability>`: the tasks need no confirmation.
5. `/gsdd impl <capability>`: by default, one task per run. Review its changes, commit them, and run it again for the next task.
6. `/gsdd verify <capability>`: try the change yourself, and accept it.

Each phase's subcommand ends by suggesting the next command. In the same conversation, you can also ask for the next step in your own words, such as "go on with the design".

For a bug, describe it with `/gsdd <the bug>`, or start with `/gsdd debug <the symptom>`. When the cause lies in the code, it is fixed with a test that cites the broken criterion; when it lies in the architecture, the design, or the requirements, the capability goes back to that phase.

## Documents

gsdd writes its documents under `docs/` in your project. They belong to the project: commit them with the code. They are plain documents that stand on their own, so the project can go on without gsdd.

```
docs/
├── steering/                   project memory
│   ├── settings.json           settings
│   └── *.md                    product.md, tech.md, architecture.md, research.md, and topic files you agree to
└── specs/
    └── <capability>/           one directory per capability
        ├── spec.json           its phase
        ├── requirements.md     why it exists, its scope, and its requirements with acceptance criteria
        ├── design.md           how it is built: its structure, interfaces, and decisions
        ├── research.md         the evidence behind its decisions
        └── tasks.md            the tasks of the latest change, and how it was verified
```

Each acceptance criterion has an ID such as `2.3`. A test that verifies a criterion cites it as `<capability> <ID>`, for example `// file-transfer 2.3`, so that the tests and the requirements can be checked against each other. The requirements hold only what is current, numbered without gaps: a criterion dropped is deleted, and its number goes to a new criterion or is closed by renumbering the ones after it; the documents that cite them are updated at once. The code, tests included, follows when the change is implemented. Git keeps the history.

## Settings

`docs/steering/settings.json` is created with these defaults:

```json
{
  "language": "zh",
  "impl": {
    "auto_commit": false,
    "tasks_per_run": 1
  }
}
```

| Field | Default | Meaning |
|---|---|---|
| `language` | `zh` | The language gsdd replies and writes its documents in, as a code such as `en` |
| `impl.auto_commit` | `false` | When on, each task is committed once its review passes: only its own files, and never pushed. When off, you review and commit each task |
| `impl.tasks_per_run` | `1` | With `auto_commit` on, the most tasks one `/gsdd impl` run does. With it off, a run does one task, so that each task stays one commit |

## Scope

One `docs/` describes one system in one repository. gsdd does not cover a system spread across several repositories, the requirement hierarchies and traceability that certification demands (such as in automotive, aviation, or medical software), or exploratory work before the direction is clear: experiment first, and create capabilities once it is clear what to build.

## Notes

- **Typed commands only.** gsdd never starts on its own: run it by typing `/gsdd`.
- **Claude Code only.** Other tools read skills from other directories, with a different header format.
- **No pushes, and no commits by default.** gsdd commits only with `impl.auto_commit` on, and never pushes.
- **Independent checks cost tokens.** The review of the architecture, of each task, and of a design that is not small, and any check of work written earlier in the same conversation, run in fresh subagents, so that the author's reasoning does not steer them.

## Modifying gsdd

```
SKILL.md                 entry point: language, paths, and routing to a subcommand
<subcommand>/guide.md    what the subcommand does, starting with a "Read first" list of the shared files it needs
req/ears.md              how acceptance criteria are written
design/principles.md     what a design covers
design/review.md         how a design is reviewed
arch/architecture.md     what the architecture covers, and how it is reviewed
shared/                  rules that several subcommands share: principles, documents, phases, conventions, capabilities, steering
```

When you change it:

- **Keep each rule in one place.** `SKILL.md` only routes. A rule for one subcommand goes in its guide; a rule that several subcommands share goes in `shared/`, and each guide that needs it lists the file in its "Read first" line.
- **Paths are relative to the gsdd directory**, except those starting with `docs/`, which are the project's.
- **A subagent does not see `SKILL.md`.** Wherever a guide hands work to a subagent, pass the gsdd directory and the language along with the work.
- **Only the conventions are fixed.** Documents have no templates. The formats in `shared/conventions.md` and the fields of the JSON files are relied on by other subcommands and by the tests that cite criteria; change them everywhere at once.
- **Test every change.** The guides are prompts, and only running them shows what they do. Set up a small sample project, install your copy at `.claude/skills/gsdd`, run a subcommand, for example `claude -p "/gsdd task <capability>"`, and compare the results before and after the change.

## License

gsdd is modified from cc-sdd and keeps its MIT license and copyright notice: see [LICENSE](LICENSE).
