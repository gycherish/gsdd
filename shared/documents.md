# Documents

gsdd's documents live in the project's `docs/` directory:

```
docs/
├── steering/                   project memory
│   ├── settings.json           settings
│   └── *.md                    product.md, tech.md, structure.md, and topic files
└── specs/
    └── [<group>/]<capability>/ one directory per capability
        ├── spec.json           the capability's phase
        ├── requirements.md
        ├── design.md           only when a design is needed
        ├── research.md
        └── tasks.md
```

- **Steering** holds what applies to the whole project and cannot be learned by reading the code. Subcommands that write it follow `shared/steering.md`.
  - `product.md`: the product's purpose, its users, and its scope.
  - `tech.md`: technology choices and constraints, technical decisions with their reasons and the alternatives rejected, and the commands to build, test, and run.
  - `structure.md`: how the code is organized, and its naming and other conventions.
  - Topic files: one topic each, such as testing.
  - `settings.json`: exactly one field, `language`.
- **Capabilities**: each directory under `docs/specs/` that holds a `spec.json` is a capability, and the directory name is the capability's name. A group folder holds capability directories and no documents.
- **requirements.md**: why the capability exists, its goals and scope, the capabilities it depends on, and its requirements with acceptance criteria.
- **design.md**: what implementers and reviewers need to build and check the capability: its structure, interfaces, data, decisions with their reasons, and the risks and unverified assumptions it rests on.
- **research.md**: the evidence behind the capability's decisions, in its requirements and its design, so that they can be traced and checked again when conditions change. It exists once there is something to record, and every subcommand that investigates follows these rules:
  - Organize it by question. For each: the question; the evidence, such as compared alternatives, primary sources with their versions, experiments with their environment, method, and result, or the root-cause analysis of a bug; the conclusion; and the decision it supports.
  - requirements.md and design.md state their decisions themselves. research.md holds the comparisons and the evidence, which they do not repeat.
  - Leave out investigation logs, findings that support no decision, and what the code shows.
  - Experiment code is thrown away unless the project keeps its experiments: record enough of the method to repeat it, and where the code is when it is kept.
  - When a decision changes, update its evidence. Remove what no longer supports a current decision, but keep a rejected alternative while it still explains a current choice.
- **tasks.md**: how the latest change was broken into tasks, and how it was verified.
- Documents are edited in place, and git keeps their history. They carry no change logs, version notes, or edit dates.
- Documents have no templates: choose sections, headings, and order to fit the content. Only the formats in `shared/conventions.md` and the fields of the JSON files are fixed.
