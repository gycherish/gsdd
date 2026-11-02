# Design Review

Review a design draft as its implementers and reviewers will use it, against the capability's requirements, the steering, and the code. Report at most three problems, the ones that matter most, and nothing about style. For each problem: what is wrong, why it matters, what to change, the acceptance criteria it affects, and where it is in design.md.

Check that:

- **Coverage**: every requirement is served, and every acceptance criterion can be verified as the design describes.
- **Fit**: the design follows the existing architecture, the steering, and the project's conventions, and respects the dependency direction it sets.
- **Readiness**: the parts and contracts are concrete enough to split into tasks and implement without guessing, and every unverified assumption is listed with how it will be checked.
- **Size**: no part, layer, or abstraction exists for needs the requirements do not have.
- **Failures**: the failures that matter are handled and logged.
- **Requirements**: the requirements have no contradiction or gap, and no part should be a capability of its own.

End with a verdict: ready for tasks, or not ready, with the problems that block it.
