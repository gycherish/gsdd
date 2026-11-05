# Conventions

Other subcommands and tools rely on these formats; follow them exactly.

1. **Acceptance criteria.** Each acceptance criterion can be checked by a test or an observation, and has an ID `N.M`: `N` is the number of its requirement, and `M` numbers the criteria under it, both from 1 and without gaps. Git keeps the history, so the requirements hold only what is current:
   - A criterion is edited in place and keeps its ID, whether or not its meaning changes.
   - A requirement or criterion that is dropped is deleted.
   - A new criterion takes the number of a criterion dropped from the same requirement, and a new requirement that of a dropped requirement; otherwise each takes the next number.
   - The numbers still left empty are closed by renumbering the ones after them.

   The citations in the documents, such as design.md and those of other capabilities, are updated with the requirements, wherever they appear: a citation of a dropped criterion is removed, and one whose number changed is renumbered. Everything else in the documents, such as what the designs say, changes in its own phase. The code, including its tests and scripts, is brought in line with the new requirements when the change is implemented (`task/guide.md`); git shows how they changed, such as a number that now belongs to a new criterion.
2. **Test citations.** A test that verifies an acceptance criterion cites it as the exact text `<capability> <ID>` (for example, `file-transfer 2.3`) in a comment or in the test's description. A criterion that cannot be tested automatically gets a manual check task in tasks.md.
3. **Tasks.** Each task in tasks.md has a checkbox (`- [ ]`, and `- [x]` when done), a number, and the IDs of the acceptance criteria it serves; a task that only prepares for others, such as setting up the project, may have none.
4. **Dependencies.** requirements.md lists the capabilities it depends on, each written as `<capability>: <behavior>`, naming the behavior it needs from that capability. A capability depends on exactly the capabilities whose behavior its criteria need: behavior that their requirements hold, and without which its criteria cannot be met or checked. Dependencies go one way, so that the capabilities can be put in an order in which each comes after those it depends on.
