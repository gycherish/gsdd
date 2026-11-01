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

- **Steering** holds what applies to the whole project and cannot be learned by reading the code: the product's purpose, users, and scope; technology choices and constraints; decisions with their reasons and rejected alternatives; conventions; and the commands to build, test, and run. Only the `steering` subcommand writes it; other subcommands hand it the decisions to record.
- **settings.json** holds exactly one field, `language`.
- **Capabilities**: each directory under `docs/specs/` that holds a `spec.json` is a capability, and the directory name is the capability's name. A group folder holds capability directories and no documents.
- **requirements.md**: why the capability exists, its goals and scope, the capabilities it depends on, and its requirements with acceptance criteria.
- **design.md**: what implementers and reviewers need to build and check the capability: its structure, interfaces, data, decisions with their reasons, and the risks and unverified assumptions it rests on.
- **research.md**: the investigations, sources, compared alternatives, and experiment results behind design.md.
- **tasks.md**: how the latest change was broken into tasks, and how it was verified.
- Documents are edited in place, and git keeps their history. They carry no change logs, version notes, or edit dates.
- Documents have no templates: choose sections, headings, and order to fit the content. Only the formats in `shared/conventions.md` and the fields of the JSON files are fixed.

## Language

Read `language` from `docs/steering/settings.json` (a language code such as `zh` or `en`), and think, respond, and write every file, headings included, in that language. When the file or the field is missing, use Simplified Chinese. Code identifiers, paths, commands, quoted text such as error messages, and wording fixed by a convention (such as the English EARS keywords) stay as they are.
