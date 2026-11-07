# impl

Read first: `shared/principles.md`, `shared/documents.md`, `shared/instructions.md`. Before choosing technology, also read `shared/technology.md`.

This command implements the product from its documents.

## 1. Choose What to Build

- Build what the user names. When the user names nothing, choose the next piece of the version being built from the documents and the code, and say what you chose and why.

## 2. Read

- Read `docs/README.md`, and the documents that cover the piece: the requirements of its features, the architecture and `tech.md`, `ux.md` and `style.md` when it has a user interface, and its design and the designs of the parts it uses.
- When the piece needs a design (`design/guide.md`) and has none, write it first, by that guide up to its Confirm step. When the design holds choices that are the user's (`design/guide.md`), put them to the user, and go on once they decide; otherwise go on at once, and list the design in the report.

## 3. Build

- How to split the work, and how to write, test, and check it, is your judgment.
- The code follows the architecture and `tech.md`. An item of technology the piece needs that `tech.md` does not name is put to the user first (`shared/technology.md`).

## 4. Keep the Documents True

- When the work changes what the documents state about how the product is built (its modules, interfaces, data, and commands), update the documents in the same change (`shared/documents.md`).
- A change to a decision that is the user's (`shared/principles.md`) is put to the user first, with its reason; change the code and the documents once they agree.

## 5. Report

- Report, in the language of `SKILL.md`, what was built, how it was checked, and the documents updated, with a suggested commit message that states what changed and why (`shared/documents.md`). The user reviews and commits.
- Suggest the next piece to build.
