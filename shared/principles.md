# Principles

gsdd writes the documents of a new project before its code, and keeps them true as the code changes: the requirements, the user interface, the architecture, and the detailed design. They record what the product is, how it is built, and why, for the people and the AI who build it.

1. **The documents alone rebuild the product.** The requirements with the user interface documents (`docs/product/`, `docs/ui/`), and the architecture (`docs/architecture/`), are complete enough that an AI given only them, without the code, the designs, or the conversations behind them, can build the product again: the same features, behavior, interface, and shape. They follow established templates, so that any reader knows where to find each thing (`req/srs.md`, `arch/architecture.md`). The designs record how the parts that need it are built.
2. **Research before deciding.** The features come from the products of the same kind and from what their users need; the technology and the designs come from current practice and primary sources (`shared/research.md`). What the research, the documents, and the user's words cannot settle is asked, and never assumed.
3. **The user decides what binds the product.** These decisions are the user's: the product's form and the features of each version, the features that would change the architecture's shape among them (`req`); the visual style (`ui`); and the technology, whenever an item of it is chosen (`shared/technology.md`). Put each such decision to the user with your recommendation and its reason, and write it once they decide. Decide everything else yourself, and list what you decided in the summary.
4. **Documents state current facts.** They say what the product is now, and are edited in place as it changes (`shared/documents.md`).
5. **The work itself is free.** How to split the work, and how to implement, test, debug, and review it, is the judgment of the AI and the user at the time. gsdd's rules cover the documents and the decisions that are the user's.

## Working Rules

- Only `impl` changes the product code. gsdd never commits or pushes: the user reviews and commits.
- The subagents that the guides call for are part of what the user asked for by running gsdd: dispatch them.
- gsdd's rules take precedence over other skills in what they define: the documents, their places, and the decisions that are the user's.
