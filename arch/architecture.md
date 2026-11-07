# Architecture

`docs/architecture/architecture.md` follows arc42, the template for architecture documentation by Gernot Starke and Peter Hruschka (https://docs.arc42.org). Keep the sections the system needs, in this order and with their numbers; leave out those that do not apply; and add any section the system needs. What each section says, and how, is your judgment.

It covers the version being built, and decides what concerns the whole system: what several parts rely on, or what would be costly to change once they are built. What one part can decide alone is left to that part's design.

| Section | Holds |
|---------|-------|
| 1 Introduction and goals | A short overview of the requirements, referring to `requirements.md`; the three to five quality goals that shape the architecture most, chosen from the nonfunctional requirements of `requirements.md` and referred to by their names; and the stakeholders other than the users, when there are any. |
| 2 Constraints | The technical and organizational constraints the architecture keeps, from the requirements and the user's choices, and the conventions it follows. |
| 3 Context and scope | The system as a black box: its users and the external systems it exchanges data with, and the channel and the protocol of each exchange. |
| 4 Solution strategy | The fundamental decisions: the technology, with its details in `tech.md`; how the system is decomposed; and how the quality goals are reached. |
| 5 Building block view | The processes and their modules, level by level as far as the whole system needs: what each is responsible for, its interfaces, and the allowed dependency direction; the modules each feature area of the version lands in; and how the code is organized, by the modules. The modules are named as `shared/documents.md` says. |
| 6 Runtime view | How the building blocks work together in the main scenarios, and at startup, shutdown, and failure. |
| 7 Deployment view | Where each process runs, and how it is installed, started, configured, and updated. |
| 8 Crosscutting concepts | The approaches every module follows: the domain model shared across modules, persistence and data formats, concurrency, error handling, logging, configuration, security, and how the user interface is built. |
| 9 Architecture decisions | The important decisions, each with its reason and the main alternative rejected. |
| 11 Risks and technical debt | The risks and the unverified assumptions, each with how it will be checked first. |
| 12 Glossary | The technical terms; the product's terms are in `requirements.md`. |

Section 10, the quality requirements, is left out: they are in section 5 of `requirements.md`, and sections 1, 4, and 8 refer to them there.

The interfaces between processes, and the data formats that processes share or persist, are set here in the detail an implementer needs. Interfaces within a process belong to the designs of the parts that provide them.

## Diagrams

Draw these, each in its section and next to the text it explains (`shared/documents.md`):

- **Context** (3): the system, its users, and the external systems, with the channels between them.
- **Building blocks** (5): the processes and modules, and their allowed dependencies.
- **Main flows** (6): a sequence diagram for each main scenario that involves three or more processes or services, or, in a system of one process, three or more modules: who calls whom, and for what.
- **Deployment** (7): where the processes run, when there are several processes or machines.

## Review

Review the architecture as the designers and implementers of its parts will use it, against the requirements, the user interface documents, and the code. Report at most three problems, the ones that matter most, and nothing about style. For each: what is wrong, why it matters, and what to change.

Check that:

- **Reproducible**: with the requirements and the user interface documents, an implementer can build the system in this shape, from its technology to its modules, without asking anything that concerns the whole system.
- **Coverage**: every feature of the version being built lands somewhere, and each main scenario of the requirements can be traced end to end through the parts. Each nonfunctional requirement is reached by the solution strategy or a crosscutting concept. The screens and states of `ux.md` are supported.
- **Boundaries**: each piece of data and behavior has one owner, and the dependencies go one way, without cycles.
- **Size**: every process, module, and layer serves a need of the version being built.
- **Level**: it settles what the whole system needs, and leaves to the designs what one part can decide.
- **Assumptions**: each technical fact the architecture rests on is checked, or listed as a risk with how it will be checked first.
- **Diagrams**: the diagrams above are drawn, and agree with the text.

End with a verdict: ready for the designs, or not ready, with the problems that block it.
