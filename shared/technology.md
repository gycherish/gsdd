# Choosing Technology

The technology is the user's to choose: the language and its version, the build system, the package management, the third-party libraries, and the code conventions. `arch` chooses it for the project; `design` and `impl` put to the user, the same way, each new item they need.

1. Research each item (`shared/research.md`): the options in current use; what established comparable products and open-source projects use, and why; and each option's maturity, maintenance, license, support for the target platforms, and known issues.
2. Put the items to the user as one list. For each: the options; what the research found, each finding with its source; the reasons for and against each option; and your recommendation, with the reason when it departs from current practice. The list covers every item the project needs, the version of the language included. A choice the user has already made is recorded, and not asked again.
3. Write each item into `docs/architecture/tech.md` once the user chooses it, with the commands to build, test, and run that follow from the choices. An answer that accepts your recommendations covers the items on the list; an item that comes up later is put to the user the same way.
