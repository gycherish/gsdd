# Architecture

`docs/steering/architecture.md` sets the direction of the system as a whole, so that every capability's design fits into one shape. Design it top-down, from the whole system to its parts, before any capability is designed.

## What It Holds

It decides only what concerns the whole system: what several capabilities rely on, or what would be costly to change once they are built. Everything else is decided in the design of the capability that needs it, within this shape. Choose its sections for this project; a simple project may need only a few lines. Cover each of these when it applies:

- The processes, and how they are deployed and started.
- The modules of each process, what each is responsible for, and the allowed dependency direction.
- How the processes and modules work together: who talks to whom, and over what kind of channel.
- The mechanisms every module follows, such as the concurrency model, how errors cross process boundaries, and where the security boundaries lie: the direction each design follows, not its details.
- For each capability, the modules it lands in. A capability may span several modules, and a module may serve several capabilities.
- How the code is organized: by the modules, not by capability, unless a capability truly is a module of its own.
- The decisions behind the shape, each with its reason and the main alternative rejected.

Interfaces, message formats, data structures, and the internals of the modules belong to the designs of the capabilities that own them.

## Diagrams

Draw these, each next to the text it explains (`shared/documents.md`):

- **The system**: the processes and services, where each runs, and the channels between them. A system of one process skips it.
- **The modules**: the modules and their allowed dependencies.
- **The main flows**: a sequence diagram for each main scenario that involves three or more processes or services, or, in a system of one process, three or more modules, such as signing in or opening a connection: who calls whom, and for what. Messages and their fields belong to the designs.

## Review

Review the architecture as the designers of the capabilities will use it, against the product, the requirements of every capability, and the code. Report at most three problems, the ones that matter most, and nothing about style. For each: what is wrong, why it matters, and what to change.

Check that:

- **Coverage**: every capability lands somewhere, and each main scenario of the requirements can be traced end to end through the parts.
- **Boundaries**: no two parts own the same data or behavior, and the dependencies go one way, without cycles.
- **Capabilities**: no capability depends on another for behavior that the architecture gives to the capability itself, and the capabilities can still be built in order (`shared/capabilities.md`).
- **Size**: no process, module, or layer exists for needs the requirements do not have.
- **Level**: it settles what the whole system needs, and leaves to the capabilities' designs what one of them can decide.
- **Readiness**: each capability can be designed within it without deciding anything at the level of the whole system.
- **Assumptions**: each technical fact the architecture rests on is checked, or listed as a risk with how it will be checked first.
- **Diagrams**: the diagrams above are drawn, and agree with the text.

End with a verdict: ready for the capabilities' designs, or not ready, with the problems that block it.
