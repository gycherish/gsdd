# Design Review Gate

Before writing `design.md`, review the draft design and repair local issues until the design passes or a true spec gap is discovered.

## Requirements Coverage Review

- Every requirement must be realized by at least one component (Components table) or body element, and every acceptance criterion ID must appear in Verification.
- Every requirement that introduces an external dependency, integration point, runtime prerequisite, migration concern, observability need, security constraint, or performance target must be reflected explicitly in `design.md`.
- If coverage is missing because the design draft is incomplete, repair the draft and review again.
- If coverage cannot be completed cleanly because requirements are ambiguous, contradictory, or underspecified, stop and return to the requirements phase instead of inventing design detail.

## Body Review

- For each content type in "Choosing the Body" (`design-principles.md`) whose "include when" condition holds for this feature, the body must cover it, under whatever section names fit the feature.
- Sections are named after the feature's own concepts and contain real content; no placeholder or boilerplate sections.
- Decisions with real alternatives state why and what was rejected; decisions that hold for the whole project are marked (project-level).

## Architecture Readiness Review

- Component boundaries must be explicit enough that implementation tasks can be assigned without guessing ownership.
- Interfaces, messages, state transitions, and integration boundaries must be concrete enough for implementation and validation.
- Runtime prerequisites, migrations, rollout constraints, and failure modes must be surfaced when they materially affect implementation order or risk.

## Boundary Readiness Review

- Each component's ownership (data, state, resources) must be explicit in Components.
- If the feature relies on other specs or modules, the body must state what it uses from them and who owns the related responsibilities.
- Depends On must be concrete enough that reviewers can detect dependency violations later.
- Every exposed contract must be defined in the body section that Components names for it.
- If data, behavior, or integration responsibility appears shared across multiple components without a clear seam, stop and repair the design.
- If downstream assumptions are embedded in upstream components "for convenience," stop and repair the design.
- If the design reveals multiple independent responsibility seams that could move separately, stop and split the spec or return to roadmap discovery instead of forcing them into one spec.

## Executability Review

- The design must be implementable as a sequence of bounded tasks without hidden prerequisites.
- Parallel-safe boundaries should be visible where the architecture intends concurrent implementation.
- Avoid speculative abstraction: remove components, adapters, or interfaces that exist only for hypothetical future scope.
- Nothing an implementer would have to guess is deferred to tasks or implementation; assumptions the design relies on but has not verified are listed under Risks and Open Questions.
- If an open question blocks implementation, stop and ask the user instead of finalizing the design.

## Mechanical Checks

Before applying judgment, verify these mechanically:
- **Fixed sections present**: `Overview`, `Components`, `Change Plan`, `Verification`, and `Risks and Open Questions` appear with the template's headings.
- **Verification coverage**: Extract all acceptance criterion IDs from `requirements.md` and check that each appears in Verification. Report any IDs not found.
- **Components ↔ Change Plan**: Every component in Components has at least one Change Plan row, and every Change Plan row names a component listed in Components.
- **Exposed contracts resolve**: Every body section named in the Exposed Contracts column exists.
- **No placeholders**: Components, Change Plan, and Verification contain no "TBD", template placeholders, or empty rows.

## Review Loop

- Run mechanical checks first, then judgment-based review.
- If issues are local to the draft, repair the draft and re-run the review gate.
- Keep the loop bounded: no more than 2 review-and-repair passes before escalating a real spec gap.
- Write `design.md` only after the review gate passes.
