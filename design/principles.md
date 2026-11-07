# Design Principles

## What design.md Holds

design.md tells implementers how a part is built: the decisions and contracts that the code would otherwise leave them to guess. It states each decision itself, and leaves to the code what the code will show. Choose its sections for this part, and order them from overview to detail. Cover each of these contents when it applies:

| Content | When |
|---------|------|
| Context: what this part builds on, and the responsibilities other parts own | The part depends on other parts |
| Structure: the pieces of the part, what each owns (data, state, threads, connections), and the allowed dependency direction | The part has two or more pieces |
| Decisions: each choice, why, and the main alternative rejected | There were real alternatives |
| Interfaces and messages: the interfaces this part provides, their signatures, messages, formats, and versioning | Other parts, threads, or processes use it |
| Data: structures, invariants, persisted formats, and compatibility with data already stored | The part holds or stores data |
| Runtime behavior: flows, state machines, concurrency, startup and shutdown, and timeouts | Behavior unfolds over time or runs concurrently |
| Failures: how each failure that matters is detected, reported, recovered from, and logged | The part does I/O, networking, or anything else that can fail at runtime |
| User interface: how the part realizes its screens of `ux.md` | The part has a user interface |
| Where it lands: the directories and files of the code, and build or configuration changes | Implementers would otherwise have to search for it |
| Risks and unverified assumptions, each with how it will be checked first | Always; write "none" when there are none |

These are contents to cover, not sections, and not a limit. Name the sections after the part's own concepts; merge or split the contents to fit, and add whatever else the part needs.

Name the features each part of the design serves.

## Principles

- **Boundary first.** Settle what each piece owns and depends on before the details. Each piece of data and behavior has one owner, and each piece serves its consumers through its interface.
- **Decide rather than defer.** Make every choice an implementer would otherwise have to guess. What cannot be decided yet is a risk, with how it will be resolved.
- **Contracts, not code.** Define the pieces, their contracts, and the decisions; leave algorithms and code to the implementation, except new algorithms, which can be sketched. Specify behavior by inputs, outputs, errors, and pre- and postconditions, in the project's language and conventions.
- **Smallest sufficient design.** One responsibility per piece, and as few pieces as the requirements allow. Each layer, adapter, and extension point serves a need the requirements have. Generalize an interface when the current requirements are variations of one problem.
- **Adopt before building.** Prefer established standards, maintained libraries, and platform features that fit; when building instead, say why.
- **Say each thing once.** Refer to the document or the section that already says it.

## Writing Contracts

- **Interfaces**: inputs, outputs, and errors; pre- and postconditions when they matter.
- **Messages** across process, thread, or network boundaries: name, direction, fields with their types and units, ordering and delivery guarantees, versioning, and idempotency when they are retried.
- **State machines**: states, transitions with their triggers, and what happens on an invalid transition.
- **Data**: structures and invariants; for persisted data, also its format, location, and compatibility.
- **Failures**: reject invalid input early, degrade gracefully on partial failure, and state what is logged where diagnosis needs it.

## Diagrams

Add a diagram where it explains better than text: structure (three or more pieces), deployment (several processes or machines), sequence (three or more parties), state (lifecycles), and flowcharts (branching). Draw it as `shared/documents.md` says.

## Style

Declarative and precise: state what the part does, with its numbers and units. Prefer tables and lists to long prose.
