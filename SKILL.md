---
name: gsdd
description: Spec-driven development. Writes down what the user wants, and the decisions behind it, before code; keeps the documents true as the code changes; counts nothing as done without evidence.
argument-hint: "[steering | requirements | design | tasks | impl | verify | review | debug | status | run] [arguments]"
disable-model-invocation: true
---

# gsdd

The gsdd directory is `${CLAUDE_SKILL_DIR}`. Paths of gsdd files, in this file and in every gsdd file, are relative to that directory; paths starting with `docs/` are in the project's `docs/` directory. A subagent does not see this file: when dispatching one, give it the absolute path of the gsdd directory and this rule.

## Subcommands

Arguments: $ARGUMENTS

- No arguments: `status`.
- The first word is a subcommand name: that subcommand, with the remaining words as its arguments.
- Anything else: `requirements`, with the whole text as the request.

| Subcommand | For |
|------------|-----|
| `steering` | Project memory: building it from the code, keeping it in sync, recording decisions |
| `requirements` | Every new request or change: settling the intent and writing the requirements |
| `design` | Designing a capability |
| `tasks` | Breaking a change into tasks |
| `impl` | Implementing the tasks |
| `verify` | Verifying a change and having the user accept it; checking that the documents and the code agree |
| `review` | Reviewing a change against the documents |
| `debug` | Finding the root cause of a failure |
| `status` | Where things stand, and what to run next |
| `run` | Moving capabilities through their phases |

Read `<subcommand>/guide.md` and follow it. Read another subcommand's guide only when a guide asks for it.

This holds for the rest of the conversation: when the user later asks for another subcommand, by name or by describing it, read its guide and follow it. After a context summary, read again the guide of the subcommand in progress and the files it lists.
