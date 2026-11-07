# gsdd

gsdd writes the design documents of a new project for [Claude Code](https://claude.com/claude-code), packaged as a single skill. Say what you want to build, and it researches the product, then writes its requirements, its user interface, its architecture, and the detailed design of the parts that need one, stopping for your decisions on the way. The code is then built from these documents, and they are kept true as the code changes.

It rests on these principles:

- **The documents alone rebuild the product.** The requirements, the user interface documents, and the architecture are complete enough that an AI given only them could build the same product again. They follow established templates: the requirements follow Karl Wiegers' software requirements specification template, and the architecture follows [arc42](https://arc42.org).
- **Research before deciding.** The features come from the products of the same kind and from what their users need, not from guesses. The technology and the designs come from current practice and primary sources.
- **You decide what binds the product**: its form and the features of each version, the features that would change the architecture's shape included, the visual style, and the technology. Everything else is decided for you, and listed for you to check.
- **Documents state current facts.** They describe the product as it is now, and are edited in place; git keeps the history, and the reasons for a change go in its commit message.
- **The work itself is free.** How the work is split, implemented, tested, debugged, and reviewed is left to the AI and you.

gsdd started from [cc-sdd](https://github.com/gotalab/cc-sdd) 3.1.0.

## How It Works

```
req ─(you decide)→ ui ─(you decide)→ arch ─(you decide)→ design ⇄ impl, piece by piece
```

- `req` researches the products of the same kind and what their users need, telling you the findings of each question as it is answered. It then shows you the whole feature set, merged from the products and the research, and stops for you to decide the product's form and the features of the first version (the MVP), along with the features left out of it that would change the architecture's shape if added later. Once you decide, it writes the requirements, with the features of the MVP specified in full, and stops again for you to confirm them.
- `ui` designs the screens, the flows, and the visual style, with an HTML prototype you can open in a browser. It stops for you to choose the style. A product without a user interface skips it.
- `arch` researches the technology, and designs the architecture for the features of the version being built. It stops for you to choose the technology.
- `design` writes the detailed design that the next piece to build needs, and skips the simple parts: the product is designed piece by piece, as it is built. It changes the earlier documents only with your agreement, and stops for the choices that are yours.
- `impl` builds what you name, or the next piece of the version, from the documents, writing first the design it needs when it has none, and updates the documents when the work changes what they state.

Each step goes on to the next once you confirm it, up to the design of the first piece. `impl` starts only when you run it.

A later request that adds or changes features starts again at `req`, and goes on only to the steps whose documents it affects.

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
/gsdd <what you want to build>    a new product, or a feature to add or change, in your own words
/gsdd <command> [arguments]
/gsdd                             where the documents stand, and what to run next
```

| Command | What it does |
|---|---|
| `req` | Researches the product, writes the requirements, and plans the features of each version; `/gsdd <what you want>` runs it |
| `ui` | Designs the screens, the flows, and the visual style, with an HTML prototype |
| `arch` | Chooses the technology, and designs the architecture of the version being built |
| `design` | Designs how a module or a feature is built, for those that need it |
| `impl` | Builds the product from its documents |

gsdd starts from an empty project; starting from existing code is not supported. The documents are written in Simplified Chinese, unless you ask for another language.

## Documents

gsdd writes its documents under `docs/` in your project. They belong to the project: commit them with the code. They are plain documents that stand on their own, so the project can go on without gsdd.

```
docs/
├── README.md                index of the documents
├── product/
│   ├── requirements.md      the software requirements specification
│   └── research.md          research on comparable products and the domain
├── ui/                      for a product with a user interface
│   ├── ux.md                the screens, the flows, and the states of each screen
│   ├── style.md             the visual style and the common interactions
│   └── prototype/           HTML pages that show the screens
├── architecture/
│   ├── architecture.md      the architecture, in the arc42 structure
│   ├── tech.md              the technology, the code conventions, and the commands
│   └── research.md          research on the technology and the architecture
└── design/
    └── <part>/              one directory per module or feature that has a design
        ├── design.md
        └── research.md
```

gsdd also writes one section of the project's `CLAUDE.md`, which tells the AI working on the code which documents to read, to update them with the code, and to ask you before changing the features, the technology, or the visual style.

## Notes

- **Typed commands only.** gsdd never starts on its own: run it by typing `/gsdd`.
- **Claude Code only.** Other tools read skills from other directories, with a different header format.
- **No commits, and no pushes.** gsdd lists the files it changed and suggests a commit message; you review and commit.
- **Research costs tokens.** Broad research runs in parallel subagents, and the architecture and the larger designs are reviewed by subagents with a fresh context.

## Modifying gsdd

```
SKILL.md                 entry point: language, paths, the commands, and their order
<command>/guide.md       what the command does, starting with a "Read first" list of the files it needs
req/srs.md               the outline of the requirements
ui/guide.md              the user interface documents and the prototype
arch/architecture.md     the outline of the architecture, and how it is reviewed
design/principles.md     what a design covers
design/review.md         how a design is reviewed
shared/                  rules that several commands share: principles, documents, research, technology, and the CLAUDE.md section
```

When you change it:

- **Keep each rule in one place.** `SKILL.md` only routes. A rule for one command goes in its guide; a rule that several commands share goes in `shared/`, and each guide that needs it lists the file in its "Read first" line.
- **Paths are relative to the gsdd directory**, except those starting with `docs/`, which are the project's.
- **A subagent does not see `SKILL.md`.** Wherever a guide hands work to a subagent, pass the gsdd directory and the language along with the work, and the rule that a subagent dispatches no subagents of its own.
- **Test every change.** The guides are prompts, and only running them shows what they do. Install your copy at `.claude/skills/gsdd` in an empty project, run a command, for example `claude -p "/gsdd <a product idea>"`, and compare the results before and after the change.

## License

gsdd is modified from cc-sdd and keeps its MIT license and copyright notice: see [LICENSE](LICENSE).
