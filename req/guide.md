# req

Read first: `shared/principles.md`, `shared/documents.md`, `shared/research.md`, `shared/instructions.md`, `req/srs.md`.

The request is the arguments, or what the user has said in this conversation. With no request, ask what the user wants to build or to change. When the user confirms requirements that are already written, go straight to Confirm.

This command writes documents only.

## 1. Understand the Situation

- Read `docs/README.md` and the documents it lists. A project without `docs/product/requirements.md` is new.
- gsdd starts from an empty project: when a new project already holds product code, say that gsdd works only on new projects, and stop.
- Read the material the user points to.
- In an existing project, this command handles the requests that add features, change them, or plan a version. A request that keeps the features as they are belongs to the command whose documents it changes, `ui`, `arch`, or `design`, or to `impl`: say so, and follow that command's guide.
- When the request does not say enough about the product to know what to research, first ask what the product is for and who uses it. Ask at most four questions at a time, the most important first.

## 2. Research the Product

Research the product for a new project, and for each feature that `docs/product/research.md` does not cover yet (`shared/research.md`):

- Find the products of the same kind, commercial and open-source: start from those their users choose most, and add more while they bring features or product forms not found yet. Read what each does in its official documentation, feature lists, and release notes, and what its users value, miss, and complain about in reviews, forums, and issue trackers.
- When there are no products of the same kind, research instead the products that serve neighboring needs, how people do the task today, the standards and regulations that apply, and published studies and community discussions of the problem.
- Compare the features, and the forms the products take: their platforms, whether they work alone or connected, who uses them, and how they are deployed.
- Record the research in `docs/product/research.md`.

## 3. Plan the Features

Merge the features of the products and the needs the research found into the feature set (`req/srs.md`), and plan the version:

- Each feature comes from the research or from the user. Where products do the same thing in ways that conflict, keep the way that fits the product's purpose and form, and put the choice to the user.
- For the MVP, recommend the smallest set of features that serves the product's purpose.
- Among the features not planned for the version, find those that would give the architecture another shape if they were added later. For each, say what it would change, and what adding it later would cost, and recommend whether to plan it for the version. The architecture is then designed for the features of the version alone.

Before writing the requirements, present, in the language of `SKILL.md`: the products researched and what each shows; and the feature set, by area, each feature with its source, or for a change, the features it adds or changes. Then put to the user, as one numbered list, each with your recommendation and its reason: the product's form; each choice between ways that conflict; the features of the version, each on its own; and the features that would change the architecture's shape. Ask the user to decide, and write the requirements once they do.

## 4. Write the Requirements

Write `docs/product/requirements.md` by `req/srs.md`, as the user decided. It describes the product as its users and operators see it.

- A feature planned for a version is described fully enough that its interface, the architecture, and its code are built without guessing (`shared/principles.md`).
- When the user leaves a point to you, recommend an answer with its reason; it holds once the user confirms it.
- A change edits `requirements.md` in place. Planning a version marks its features, and gives each of them its section in 4.

With the first documents, also write `docs/README.md` (`shared/documents.md`) and the section of `CLAUDE.md` (`shared/instructions.md`).

## 5. Check

Before presenting the requirements, check that:

- Every feature comes from the research or from the user, and every need the research found has its feature.
- Every feature planned for a version is described fully enough to build without guessing.
- Each statement comes from the research, the user, or the documents.
- The requirements are self-contained (`shared/documents.md`).

## 6. Confirm

- **After writing**: present, in the language of `SKILL.md`, a summary of the requirements: the features of the version and what each does, and the details you decided yourself. Then put to the user, as one numbered list, each with your recommendation and its reason, the points they left to you and any decision of theirs that the writing raised. Ask the user to confirm.
- **When the user confirms**, apply their answers, and go on at once (`SKILL.md`) to `ui` when the product has a user interface, otherwise to `arch`. After a change, go on to the first of `ui`, `arch`, and `design` whose documents the change affects, and say why; when it affects none of them, suggest `/gsdd impl`.
