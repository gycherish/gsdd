# Documents

gsdd's documents live in the project's `docs/` directory:

```
docs/
├── README.md                index of the documents
├── product/
│   ├── requirements.md      the software requirements specification
│   └── research.md          research on comparable products and the domain
├── ui/                      for a product with a user interface
│   ├── ux.md                the layout, the screens, the flows, and the states of each screen
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

- `README.md`: one line per document: what it holds, and the work it serves, so that whoever works on a part of the product knows which documents to read first. Update it whenever a document is added or removed.
- `requirements.md`: `req/srs.md`. `ux.md`, `style.md`, and the prototype: `ui/guide.md`. `architecture.md`: `arch/architecture.md`. `design.md`: `design/principles.md`.
- `tech.md`: the technology the user chose and its constraints, the code conventions, and the commands to build, test, and run (`shared/technology.md`).
- Each `research.md`: the evidence behind the decisions of the documents next to it (`shared/research.md`). It exists once there is something to record.
- The project's `CLAUDE.md` holds a section that tells the AI working on the code how to use these documents (`shared/instructions.md`).

## Writing

- **Current facts.** Every document states what the product is now: what it does, how it is built, and the reason for each current choice. A change edits the text in place, so that the documents read as a description of the product as it is; git keeps the history, and the reasons for the change go in its commit message.
- **What the code cannot tell.** Write intent, decisions and their reasons, contracts, constraints, and conventions. What reading the code shows is left to the code.
- **Self-contained.** `requirements.md`, the user interface documents, `architecture.md`, and `tech.md` state in their own text every fact, reason, and source they rest on. Among the project's documents, each refers only to those of them it builds on: `requirements.md` to none; the user interface documents to `requirements.md` and to one another; `architecture.md` to `requirements.md`; and `tech.md` to `requirements.md` and `architecture.md`. They may cite public sources outside the project, with what they take from each stated in the text. The other documents may refer to any document.
- **One file per document**, at the path the tree above shows; the prototype is the pages of its directory. The requirements and the architecture follow their outlines (`req/srs.md`, `arch/architecture.md`); the other documents choose their sections, headings, and order to fit the content.
- **Paths.** Every directory and file in `docs/`, `README.md` aside, is named in lowercase English words joined by hyphens, whatever language the documents are written in.
- **Diagrams** are plain Mermaid, next to the text they explain: node IDs of letters, digits, and underscores, and labels without parentheses, brackets, quotes, or slashes.
- **Secrets** (keys, passwords, credentials, and internal addresses) are never written.
- After changing documents, tell the user which files changed, and give a suggested commit message that states what changed and why.

## Names

`requirements.md` names the features, and `architecture.md` the processes and the modules; every document calls them by those names. A feature or a module is named this way:

- The name says what the part acts on, then what it does, the action written as a noun: its lasting behavior or responsibility, without versions or dates. A feature that the product itself names, and its users know by that name, keeps that name when it stands for one behavior.
- Each word has a clear meaning, in whichever language the name is written. `management`, in any language, says nothing of what a part does, and is never used.
- A part that no clear action fits holds several parts, and is split by what changes together.
- A reader who knows the product, but not its documents, can tell from the name which part it is, and tell it apart from the others: a word that could mean several parts is qualified by what it applies to.
- Names are unique across the project.
- An English name is two or three lowercase English words joined by hyphens. Every module has one, and its code follows it in the form its language uses. A feature named in another language gets one when it has a directory of its own under `docs/design/`, written next to its name in `requirements.md`. The directory takes the English name.
- Renaming, splitting, merging, or removing a part updates every reference to it in the documents at once.
