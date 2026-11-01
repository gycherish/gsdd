# Conventions

Other subcommands and tools rely on these formats; follow them exactly.

1. **Acceptance criteria.** Each acceptance criterion can be checked by a test or an observation, and has an ID `N.M`: `N` is the number of its requirement, and `M` numbers the criteria under it. Numbers are never changed or reused: a new requirement or criterion takes the next unused number, and a deleted one keeps its number, marked as deleted. A criterion edited without changing its meaning keeps its ID; when its meaning changes, mark it deleted and add the new one under the next unused number, so that an ID always means one thing.
2. **Test citations.** A test that verifies an acceptance criterion cites it as the exact text `<capability> <ID>` (for example, `file-transfer 2.3`) in a comment or in the test's description. A criterion that cannot be tested automatically gets a manual check task in tasks.md.
3. **Tasks.** Each task in tasks.md has a checkbox (`- [ ]`, and `- [x]` when done), a number, and the IDs of the acceptance criteria it serves.
4. **Dependencies.** requirements.md names the capabilities it depends on by their names.
