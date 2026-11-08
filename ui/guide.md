# ui

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `req/srs.md`.

This command designs the user interface of the version being built, every screen of it, and records it in `docs/ui/`: `ux.md`, `style.md`, and the prototype. For a product without a user interface, say so, and go on to `arch`.

This command writes documents only.

## 1. Understand the Situation

- Read the requirements: the product's users, form, and platforms, and the features of the version being built.
- Read the documents of `docs/ui/` and `docs/architecture/` that exist. When they exist, change only what the request or the changed requirements affect, and keep the rest.
- The design is made in the tool the user names; otherwise, it is the prototype itself: HTML pages in `docs/ui/prototype/`, each holding its own styles and scripts, opening directly in a browser without a network or a build. A design kept in a tool outside the project has its link in `docs/README.md`, and a change is made in that design.

## 2. Research

When the design needs it, research how products of the same kind present the same features, the conventions users of the domain expect, and the interface guidelines of the target platforms (`shared/research.md`). What it finds about products of the same kind goes into `docs/product/research.md`; the platform guidelines the interface follows are named in `style.md`, with their links.

## 3. Design Step by Step

The interface is designed in steps, and the user confirms each step before the next one begins: the layout of the main screen, the visual style, what the main screen holds, and then each other screen, one at a time. A step builds on those confirmed before it.

For each step:

- Make it in the design, from the requirements.
- What it shows a feature needs, and the requirements do not have yet, in its behavior, its states, or its responses to errors, is an addition to the requirements.
- Present, in the language of `SKILL.md`, where to open the step in the design, and what it shows. Put to the user, as one numbered list, each with your recommendation and its reason: the choices the step leaves open, and its additions to the requirements. Ask the user to confirm.
- When the user confirms, apply their answers in the design, write the additions they agree to into the requirements, as behavior of the features (`req/srs.md`), record the step in `docs/ui/` (below), and go on at once to the next step.

### The Layout

The layout of the main screen, the screen users see most: how its window is divided into regions, each region drawn empty. Show its real alternatives side by side.

With the layout, present the screens of the version being built, each with the features it serves, and the main flows across them, from start to end, so that the user sees what the layout must hold. You arrange the screens and the flows yourself, and they are recorded with the layout.

### The Visual Style

The real alternatives of the visual style, side by side on the layout, for the user to choose, with the common interactions: how actions are confirmed, and how errors, progress, and notifications are shown. A product that runs natively on a platform follows that platform's interface guidelines.

### The Main Screen

What each region of the layout holds: the features in it, and how they are arranged; what the part of each feature looks like; what users can do there, and the feedback for each action; and its states.

### The Other Screens

Each other screen of the version, one at a time, in the order you choose: what it holds, and how it is arranged; what users can do on it, and the feedback for each action; and its states: empty, loading, error, and each other state its features have.

## 4. Record the Design

Record each step in `docs/ui/` once the user confirms it, so that the project holds the whole interface on its own:

- **`ux.md`**: the layout, the screens and the flows; and for each screen designed, the features it serves, what it shows, what users can do on it with the feedback for each action, and its states.
- **`style.md`**: the visual style every screen follows: the colors, the type, the spacing, the icons, and the controls with their states; the common interactions; the accessibility requirements; and the platform guidelines it follows.
- **The prototype**: each screen designed, as HTML pages in `docs/ui/prototype/`, taken from the tool when the design is kept in one. `index.html` links to every page, and each page opens in a browser from the project. The prototype shows the design; the product's code is built in `impl`.

## 5. Finish

When the user has confirmed every screen of the version, present, in the language of `SKILL.md`, a summary: the screens and the main flows, the style, the path of `docs/ui/prototype/index.html`, and the files that changed. Go on at once (`SKILL.md`) to `arch`.

A change goes through the steps it affects: the layout or the visual style when it changes them, and then each screen it changes. Then go on to the first of `arch` and `design` whose documents the change affects, and say why; when it affects neither, suggest `/gsdd impl`.
