# Documents

gsdd's documents live in the project's `docs/` directory:

```
docs/
├── steering/                   project memory
│   ├── settings.json           settings
│   └── *.md                    product.md, tech.md, architecture.md, research.md, and topic files
└── specs/
    └── [<group>/]<capability>/ one directory per capability
        ├── spec.json           the capability's phase
        ├── requirements.md
        ├── design.md
        ├── research.md
        └── tasks.md
```

- **Steering** holds what applies to the whole project and cannot be learned by reading the code. Only the subcommands named here write it, following `shared/steering.md`.
  - `product.md`, written by `req`: the product's purpose, its users, its platforms, and its scope.
  - `architecture.md`, written by `arch`: the shape of the whole system, its processes and modules, how they work together, and how the code is organized (`arch/architecture.md`).
  - `tech.md`: the technology and its constraints, the code conventions, technical decisions with their reasons and the alternatives rejected, and the commands to build, test, and run. `arch` writes it when the project starts; later, `design` changes it for the capability that needs it, and `req` records the technical constraints the user states.
  - `research.md`, written by `req` and `arch`: the evidence behind the decisions they make for the whole project, such as research about the product, compared technologies, or experiments on the target platforms.
  - Topic files: one topic each, such as error handling, written by `arch` or `design` with the user's agreement.
  - `settings.json`: `language`, the language gsdd writes in (`SKILL.md`); and `impl`, how tasks are implemented and committed (`impl/guide.md`). A missing field takes its default. `req` creates the file when it is missing, with the defaults: `{"language": "zh", "impl": {"auto_commit": false, "tasks_per_run": 1}}`.
- **Capabilities**: each directory under `docs/specs/` that holds a `spec.json` is a capability, and the directory name is the capability's name. A group folder holds capability directories and no documents.
- **requirements.md**: why the capability exists, its goals and scope, the capabilities it depends on, and its requirements with acceptance criteria.
- **design.md**: what implementers and reviewers need to build and check the capability: its structure, interfaces, data, decisions with their reasons, and the risks and unverified assumptions it rests on.
- **research.md**: the evidence behind decisions, so that they can be traced and checked again when conditions change: a capability's for the decisions made in its requirements and its design, even when a decision is written into the steering, such as a library its design chose; and the steering's for the decisions that `req` and `arch` make for the whole project. It exists once there is something to record, and every subcommand that investigates follows these rules:
  - Organize it by question. For each: the question; the evidence, such as compared alternatives, primary sources with their versions, experiments with their environment, method, and result, or the root-cause analysis of a bug; the conclusion; and the decision it supports.
  - The other documents state their decisions themselves. research.md holds the comparisons and the evidence, which they do not repeat.
  - Leave out investigation logs, findings that support no decision, and what the code shows.
  - Experiment code is thrown away unless the project keeps its experiments: record enough of the method to repeat it, and where the code is when it is kept.
  - When a decision changes, update its evidence. Remove what no longer supports a current decision, but keep a rejected alternative while it still explains a current choice.
- **tasks.md**: how the latest change was broken into tasks, and how it was verified.
- Documents describe what the system does and what is being built; they do not mention features left out or left for later. A rule the system must keep, such as never sending data off the local network, is a requirement, not a feature left out.
- Documents are edited in place, and git keeps their history. They carry no change logs, version notes, or edit dates.
- Documents have no templates: choose sections, headings, and order to fit the content. Only the formats in `shared/conventions.md` and the fields of the JSON files are fixed.
