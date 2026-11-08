# ui

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `req/srs.md`.

This command designs the user interface of the version being built, and records it in `docs/ui/`: `ux.md`, `style.md`, and the prototype. For a product without a user interface, say so, and go on to `arch`.

This command writes documents only.

## 1. Understand the Situation

- Read the requirements: the product's users, form, and platforms, and the features of the version being built.
- Read the documents of `docs/ui/` and `docs/architecture/` that exist. When they exist, change only what the request or the changed requirements affect, and keep the rest.
- The design is made in the tool the user names; otherwise, it is the prototype itself: HTML pages in `docs/ui/prototype/`, each holding its own styles and scripts, opening directly in a browser without a network or a build. A design kept in a tool outside the project has its link in `docs/README.md`, and a change is made in that design.

## 2. Research

When the design needs it, research how products of the same kind present the same features, the conventions users of the domain expect, and the interface guidelines of the target platforms (`shared/research.md`). What it finds about products of the same kind goes into `docs/product/research.md`; the platform guidelines the interface follows are named in `style.md`, with their links.

## 3. Screens and Flows

Make the screens and the flows in the design, from the requirements:

- The screens of the version being built, and how users move between them, with the main flows from start to end.
- For each screen: the features it serves; what it shows; what users can do on it, and the feedback for each action; and its states: empty, loading, error, and each other state its features have.
- The screens are plain, without a visual style, so that the screens and the flows are judged on their own.

What the screens show a feature needs, and the requirements do not have yet, in its behavior, its states, or its responses to errors, is an addition to the requirements.

Present, in the language of `SKILL.md`: where to open the design, the screens, and the main flows. Put to the user, as one numbered list, each with your recommendation and its reason: the additions to the requirements, and the choices between screens or flows that the design leaves open. Ask the user to review the design and to confirm. When the user confirms, apply their answers in the design, write the additions they agree to into the requirements, as behavior of the features (`req/srs.md`), and go on at once to the visual style.

## 4. Visual Style

Show the real alternatives of the visual style side by side in the design, on the screens that show it best. A product that runs natively on a platform follows that platform's interface guidelines. Put the alternatives to the user, with your recommendation and its reason, and ask the user to choose. When the user chooses, apply the style to every screen of the design, and go on at once to record it.

## 5. Record the Design

Record the design in `docs/ui/`, so that the project holds the whole interface on its own:

- **`ux.md`**: the screens and the flows, as the design shows them: for each screen, the features it serves, what it shows, what users can do on it with the feedback for each action, and its states.
- **`style.md`**: the visual style every screen follows: the colors, the type, the spacing, the icons, and the controls with their states; the common interactions: how actions are confirmed, and how errors, progress, and notifications are shown; the accessibility requirements; and the platform guidelines it follows.
- **The prototype**: the screens of the design as HTML pages in `docs/ui/prototype/`, taken from the tool when the design is kept in one. `index.html` links to every page, and each page opens in a browser from the project. The prototype shows the design; the product's code is built in `impl`.

Then present, in the language of `SKILL.md`, a summary: the screens and the main flows, the style, the path of `docs/ui/prototype/index.html`, and the files that changed. Go on at once (`SKILL.md`) to `arch`. After a change, go on to the first of `arch` and `design` whose documents the change affects, and say why; when it affects neither, suggest `/gsdd impl`.
