# Capabilities

A capability is a part of the system that lasts, has its own behavior, and can be accepted on its own: a user-facing feature (`file-transfer`), an internal capability or shared foundation (`connection`, `protocol`), or a quality requirement that spans many parts (`shutdown`, `logging`). Each capability has one directory under `docs/specs/`.

- **Split by behavior, not by code structure.** A capability may span several processes or modules; how they share the work belongs in its design. Do not create capabilities per process, module, layer, or pipeline stage.
- **One owner.** Each requirement and each shared interface belongs to exactly one capability; others refer to it. An end-to-end flow belongs to the capability that is its goal, which depends on the others; a flow that is a product concept of its own is a capability.
- **Change in place.** New behavior, changed rules, and bug fixes for an existing part belong to that capability. Create a new capability only for a new part: one with its own purpose and scenarios, that will gain requirements on its own, and that can be accepted on its own.
- **No directory for work that does not change behavior.** Refactoring, dependency upgrades, build migrations, and cleanups get none: project rules they settle go to steering, and design changes go to the affected capability's design.md. If such work leaves an outcome that can be verified (for example, "the project builds with one command on a clean machine"), that outcome is a technical capability.
- **Names** are 1–3 lowercase English words joined by hyphens, naming the part, never the activity (`file-transfer`, not `add-resume-upload`), with no versions or dates. Names are unique across the project, and the directory name is the capability's name.
- **Groups.** With many capabilities, one level of group folders is allowed (`docs/specs/networking/connection/`). Group folders hold no documents, and since capability names are unique, regrouping changes no references.
- **Restructuring.** Splitting, merging, renaming, or deleting a capability updates every reference to it: dependencies in other capabilities and criterion citations in tests.
