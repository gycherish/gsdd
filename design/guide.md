# design

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `design/principles.md`. Before choosing technology, also read `shared/technology.md`.

This command designs how the parts of the product that need it are implemented: `docs/design/<part>/design.md`, with its research. The parts are those named in the arguments; with none, those that the next piece of the version to build needs, and that have no design yet: choose the piece from the documents and the code, and say which and why. The product is designed piece by piece, as it is built.

This command writes documents only.

## 1. Choose the Parts

- A part is a module of the architecture, or a feature whose implementation spans modules, called by the name the architecture or the requirements give it. Its directory under `docs/design/` takes its English name (`shared/documents.md`).
- A part needs a design when building it means making decisions that other parts or later work depend on: data or state of its own, a protocol or a format, concurrency, an algorithm of its own, a choice between real alternatives, or failures to handle. The other parts are built straight from the requirements and the architecture: list them in the summary as needing no design.
- Design the parts that others use before the parts that use them, so that each builds on the interfaces its providers define. Subagents can design independent parts in parallel: give each the gsdd directory and the language (`SKILL.md`), the path of this guide, and its part. Each writes only its part's directory, and returns the choices of Investigate that its design rests on.

## 2. Understand the Situation

For each part, read: the requirements of the features it serves; `ux.md` and `style.md` when it has a user interface; the architecture and `tech.md`; the designs of the parts it uses; its own design and research when they exist; and the code as far as needed. When its design exists, change only what the request or the changed documents affect, and keep the rest.

## 3. Investigate

- When a choice has several viable approaches, or rests on an uncertain fact, research it (`shared/research.md`): the approaches and their trade-offs, how established open-source projects and products implement the same thing, and the specifications and platform interfaces involved. Record it in the part's `research.md`.
- A third-party library or another item of technology that `tech.md` does not name is the user's choice (`shared/technology.md`), and so is any change to the requirements, the user interface documents, or the architecture. When a design needs one, design on your recommendation, mark in the design what rests on it, and put it to the user at Confirm.

## 4. Write

Write `design.md` as `design/principles.md` describes.

## 5. Review

- Check each draft against `design/review.md`.
- Unless a design is small, also have a subagent with a fresh context review it by `design/review.md`. Give it the gsdd directory and the language (`SKILL.md`), the path of `design/review.md`, and the paths of the documents and the code it needs, not your conclusions. Fix what it finds that holds, and tell the user what you did not fix and why.

## 6. Confirm

- **After writing**: present, in the language of `SKILL.md`, a summary: the parts designed and their main decisions, including those you made; the parts that need no design; and the risks and unverified assumptions. Then put to the user, as one numbered list, the choices of Investigate that are theirs, each with your recommendation and its reason. Ask the user to decide and to confirm.
- **When the user confirms**, write their answers into the documents they concern, revise the designs that rest on them, and suggest `/gsdd impl`.
