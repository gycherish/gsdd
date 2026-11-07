# ui

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `req/srs.md`.

This command designs the user interface of the version being built: `docs/ui/ux.md`, `docs/ui/style.md`, and the prototype in `docs/ui/prototype/`. For a product without a user interface, say so, and go on to `arch`.

This command writes documents only.

## 1. Understand the Situation

- Read the requirements: the product's users, form, and platforms, and the features of the version being built.
- Read the documents of `docs/ui/` and `docs/architecture/` that exist. When they exist, change only what the request or the changed requirements affect, and keep the rest.

## 2. Research

When the design needs it, research how products of the same kind present the same features, the conventions users of the domain expect, and the interface guidelines of the target platforms (`shared/research.md`). What it finds about products of the same kind goes into `docs/product/research.md`; the platform guidelines the interface follows are named in `style.md`, with their links.

## 3. Design

- **`ux.md`**: the screens of the version being built, and how users move between them. For each screen: the features it serves; what it shows; what users can do on it, and the feedback for each action; and its states: empty, loading, error, and each other state its features have.
- **`style.md`**: the visual style every screen follows: the colors, the type, the spacing, the icons, and the controls with their states; the common interactions: how actions are confirmed, and how errors, progress, and notifications are shown; and the accessibility requirements. A product that runs natively on a platform follows that platform's interface guidelines.
- **The prototype**: HTML pages in `docs/ui/prototype/` that show the main screens and their key states, in the style of `style.md`. Each page holds its own styles and scripts, and opens directly in a browser, without a network or a build. `index.html` links to every page. The prototype shows the design; the product's code is built in `impl`.
- What the design of the screens shows a feature needs, and the requirements do not have yet, in its behavior, its states, or its responses to errors, is put to the user with the decisions of this step. Once they agree, write it into the requirements.

## 4. Confirm

- **After writing**: present, in the language of `SKILL.md`, a summary: the screens and the main flows, the main style choices, and the path of `docs/ui/prototype/index.html` for the user to open. Put the visual style to the user with your recommendation and its reason; when it has real alternatives, show them side by side on one page of the prototype. Put to the user, in the same numbered list, what the design adds to the requirements. Ask the user to choose the style and to confirm.
- **When the user confirms**, apply their answers, and go on at once (`SKILL.md`) to `arch`. After a change, go on to the first of `arch` and `design` whose documents the change affects, and say why; when it affects neither, suggest `/gsdd impl`.
