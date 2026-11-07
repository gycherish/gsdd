# Project Instructions

The project's `CLAUDE.md`, at the root of the repository, holds a section that tells the AI working on the code how to use the documents. `req` writes it with the first documents, and each command whose changes make it inaccurate updates it. When `CLAUDE.md` does not exist, create it.

The section has its own heading, and is written in the language of the documents. It speaks of the documents, and names no tool, so that it holds however the code is written. It says:

- The documents are listed, with what each holds, in `docs/README.md`.
- Before working on a part of the product, read the documents that cover it: the requirements of its features, the architecture and `docs/architecture/tech.md`, the user interface documents when it has a user interface, and its design when it has one.
- When the work changes what the documents state, update them in the same change, so that they keep stating the current facts, and give the reason in the commit message.
- The features, the technology, and the visual style are the user's decisions: ask before changing them.

This section is the only part of `CLAUDE.md` gsdd writes; the rest belongs to the user.
