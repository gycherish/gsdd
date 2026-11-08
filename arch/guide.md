# arch

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `shared/technology.md`, `arch/architecture.md`, `req/srs.md`.

This command chooses the technology and designs the architecture of the version being built, and changes them when the requirements or the user interface need it. It writes `docs/architecture/`.

This command writes documents only.

## 1. Understand the Situation

- Read the requirements: the features of the version being built.
- Read the documents of `docs/ui/`, when the product has a user interface.
- Read the documents of `docs/architecture/` that exist, and the code as far as needed. When they exist, change only what the request, the changed requirements, or the changed interface need, and keep the rest.

## 2. Choose the Technology

- For the first architecture, research the technology the whole project will use, as `shared/technology.md` describes: the language and its version, the build system, the package management, the third-party libraries the architecture relies on, and the code conventions.
- For a change, research only the items it needs that `tech.md` does not have.

## 3. Design the Architecture

- Design it by `arch/architecture.md`, from the whole system down to its modules, for the features of the version being built, on the technology you recommend.
- What users will see that a choice of the architecture brings, in behavior or in quality, and that the requirements do not have yet, is put to the user with the decisions of this step. Once they agree, write it into the requirements, and, when the product has a user interface, into its design and `docs/ui/` (`ui/guide.md`).

## 4. Review

Have a subagent with a fresh context review the architecture by the Review section of `arch/architecture.md`. Give it the gsdd directory and the language (`SKILL.md`), the path of that file, and the paths of the documents and the code, not your conclusions. Fix what it finds that holds, and tell the user what you did not fix and why.

## 5. Confirm

- **After writing**: present, in the language of `SKILL.md`, a summary: the processes and modules, and where each feature area lands; the main decisions, including those you made; and the risks and unverified assumptions. Then put to the user, as one numbered list, the decisions that are theirs: the technology (`shared/technology.md`), and what users will see that the architecture brings. Ask the user to decide and to confirm.
- **When the user confirms**, write the technology into `tech.md`, and revise the architecture where a choice differs from your recommendation. Then go on at once (`SKILL.md`) to `design`. After a change, go on to `design` when the designs of the parts it affects need to change, and say why; otherwise, suggest `/gsdd impl`.
