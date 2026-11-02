# Design Principles

## What design.md Holds

design.md tells implementers and reviewers what they need to build and check the capability. It states every decision and contract itself, and does not restate what the code will show. Choose its sections for this capability, and order them from overview to detail. Cover each of these contents when it applies:

| Content | When |
|---------|------|
| Context: what exists, what this builds on, and the responsibilities other capabilities own | The change touches existing code or depends on other capabilities |
| Structure: the parts, what each owns (data, state, threads, connections), and the allowed dependency direction | There are two or more parts |
| Decisions: each choice, why, and the main alternative rejected | There were real alternatives |
| Interfaces and messages: signatures, messages, formats, and versioning | Parts, threads, or processes communicate |
| Data: structures, invariants, persisted formats, and compatibility with existing data | Data is added or changed |
| Runtime behavior: flows, state machines, concurrency, startup and shutdown, and timeouts | Behavior unfolds over time or runs concurrently |
| Failures: how each failure that matters is detected, reported, recovered from, and logged | The capability does I/O, networking, or anything else that can fail at runtime |
| User interface: layout, states, and feedback | The capability has a user interface |
| Where the change lands: modules or directories, and build or configuration changes | Implementers would otherwise have to search for it |
| Verification: how a criterion will be shown to hold | It needs more than an ordinary test, such as a harness, fault injection, or a manual check |
| Risks and unverified assumptions, each with how it will be checked first | Always; write "none" when there are none |

These are contents to cover, not sections, and not a limit. Name the sections after the capability's own concepts, such as "Session lifecycle" or "Invoice numbering" rather than "Runtime behavior" or "Data"; merge or split the contents to fit, and add whatever else the capability needs.

Cite acceptance criteria by ID (`2.1`, `2.3`) where a part of the design serves them.

## Principles

- **Boundary first.** Settle what each part owns and depends on before the details. No two parts own the same data or behavior, and no part builds in assumptions about its consumers. Parts that would change and be accepted separately are a requirement problem: the capability should be split.
- **Decide rather than defer.** Make every choice an implementer would otherwise have to guess. What cannot be decided yet is a risk, with how it will be resolved.
- **Contracts, not code.** Define the parts, their contracts, and the decisions; leave algorithms and code to the implementation, except new algorithms, which can be sketched. Specify behavior by inputs, outputs, errors, and pre- and postconditions, in the project's language and conventions.
- **Smallest sufficient design.** One responsibility per part, and as few parts as the requirements allow; no layers, adapters, or extension points for needs the requirements do not have. Generalize an interface only when the current requirements are variations of one problem.
- **Adopt before building.** Prefer established standards, maintained libraries, and platform features that fit; when building instead, say why.
- **Say each thing once.** Refer to the section that already says it.

## Writing Contracts

- **Interfaces**: inputs, outputs, and errors; pre- and postconditions when they matter.
- **Messages** across process, thread, or network boundaries: name, direction, fields with their types and units, ordering and delivery guarantees, versioning, and idempotency when they are retried.
- **State machines**: states, transitions with their triggers, and what happens on an invalid transition.
- **Data**: structures and invariants; for persisted data, also its format, location, and compatibility.
- **Failures**: reject invalid input early, degrade gracefully on partial failure, and state what is logged where diagnosis needs it.

## Diagrams

Add a diagram only where it explains better than text, next to the text it explains: architecture (three or more parts), deployment (several processes or machines), sequence (three or more parties), state (lifecycles), and flowcharts (branching). Use plain Mermaid: node IDs of letters, digits, and underscores, and labels without parentheses, brackets, quotes, or slashes.

## Style

Declarative and precise: "closes idle sessions after 30 s", not "should close idle sessions". Prefer tables and lists to long prose.
