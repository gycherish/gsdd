# Design Review

Review a design draft as its implementers will use it, against the requirements of the features it serves, the user interface documents, the architecture, `tech.md`, and the code. Report at most three problems, the ones that matter most, and nothing about style. For each problem: what is wrong, why it matters, what to change, and where it is in design.md.

Check that:

- **Coverage**: every feature the part serves is served: its main flow, its errors and edge cases, and its quality targets.
- **Fit**: the design works within the architecture: it keeps the modules' responsibilities, their dependency direction, and the mechanisms every module follows, and decides nothing at the level of the whole system. It uses the interfaces of other parts as their designs define them, and follows `tech.md` and the project's conventions.
- **Readiness**: the pieces and contracts are concrete enough to implement without guessing, and every unverified assumption is listed with how it will be checked.
- **Size**: every piece, layer, and abstraction serves a need of the requirements.
- **Failures**: the failures that matter are handled and logged.
- **Requirements**: the requirements the part serves are consistent and complete for it.

End with a verdict: ready to implement, or not ready, with the problems that block it.
