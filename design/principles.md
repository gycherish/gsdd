# Design Principles

## What design.md Holds

design.md settles, before the code, what in a part is easy to get wrong and costly when wrong, and states the reason for each:

- **What other parts depend on**: the interfaces and contracts of the part, its messages across processes, its persisted formats, the ownership of its data and threads, and how its failures show to others. Once other parts use them, changing them changes those parts too.
- **What takes thought to get right**: the key interaction flows of the part, its state machines, its concurrency, its recovery from failures, and its own algorithms. The code shows their steps; the design states why they are right, the cases they cover, and what they guarantee.

The part is built by it, and later work keeps what it states. It describes how the part works, and leaves to the code how the code is shaped, so that it holds through any rewrite of the code that keeps the part working the same way.

Choose its sections for this part, and order them from the whole part to its pieces. Cover each of these contents when it applies:

| Content | When |
|---------|------|
| Context: what this part builds on, and the responsibilities other parts own | The part depends on other parts |
| Structure: the pieces of the part, what each owns (data, state, threads, connections), and the allowed dependency direction | The part has two or more pieces |
| Decisions: each choice, why, and the main alternative rejected | There were real alternatives |
| Interfaces and messages: the interfaces this part provides, their messages, formats, and versioning | Other parts, threads, or processes use it |
| Data: what the part holds, its invariants, persisted formats, and compatibility with data already stored | The part holds or stores data |
| Runtime behavior: the key flows, state machines, concurrency, startup and shutdown, and timeouts | Behavior unfolds over time or runs concurrently |
| Algorithms: the steps of each algorithm of the part's own, the invariants it keeps, the cases it covers, and its cost in time and space | The part has an algorithm of its own |
| Failures: how each failure that matters is detected, reported, recovered from, and logged | The part does I/O, networking, or anything else that can fail at runtime |
| User interface: how the part realizes its screens of `ux.md` | The part has a user interface |
| Where it lands: the directory of the part's code, and the build or configuration changes | The architecture does not place the part |
| Risks and unverified assumptions, each with how it will be checked first | Always; write "none" when there are none |

These are contents to cover, not sections. Name the sections after the part's own concepts, and merge or split the contents to fit. Add what else the part has that other parts depend on, or that takes thought to get right.

Name the features each part of the design serves.

## Principles

- **Boundary first.** Settle what each piece owns and depends on before the details. Each piece of data and behavior has one owner, and each piece serves its consumers through its interface.
- **Decide rather than defer.** Within what a design holds, make every choice an implementer would otherwise have to guess; the shape of the code is the implementer's. What cannot be decided yet is a risk, with how it will be resolved.
- **Contracts, not code.** A design holds no code: it states the contracts, the flows, the state machines, and the algorithms in prose, steps, tables, and diagrams.
- **Smallest sufficient design.** One responsibility per piece, and as few pieces as the requirements allow. Each layer, adapter, and extension point serves a need the requirements have. Generalize an interface when the current requirements are variations of one problem.
- **Adopt before building.** Prefer established standards, maintained libraries, and platform features that fit; when building instead, say why.
- **Say each thing once.** Refer to the document, the section, or the external standard that already says it, with what the part takes from it.

## Writing Contracts

- **Interfaces**: what each operation does; its inputs, outputs, and errors; who owns what it takes and returns, and for how long; the threads it can be called from; pre- and postconditions when they matter.
- **Messages** across process, thread, or network boundaries: name, direction, fields with their types, units, and ranges, ordering and delivery guarantees, versioning, and idempotency when they are retried.
- **State machines**: states, transitions with their triggers, and what happens on an invalid transition.
- **Data**: what the part holds, and its invariants; for persisted data, also its format, field by field with their units and ranges, its location, and its compatibility.
- **Failures**: reject invalid input early, degrade gracefully on partial failure, and state what is logged where diagnosis needs it.

## Diagrams

Add a diagram where it explains better than text: structure (three or more pieces), deployment (several processes or machines), sequence (three or more parties), state (lifecycles), and flowcharts (branching). Draw it as `shared/documents.md` says.

## Style

Declarative and precise: state what the part does, with its numbers and units. Prefer tables and lists to long prose.
