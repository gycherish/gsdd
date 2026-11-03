---
name: gsdd
description: Spec-driven development. Writes down what the user wants, and the decisions behind it, before code; keeps the documents true as the code changes; counts nothing as done without evidence.
argument-hint: "[req | arch | design | task | impl | verify | review | debug | status] [arguments]"
disable-model-invocation: true
---

# gsdd

## Language

Think, reply to the user, and write every file, headings included, in Simplified Chinese, unless `language` in `docs/steering/settings.json` sets another language (a code such as `en`). This holds for every reply, however short, whatever language the request, the gsdd files, or the tool output are written in. Code identifiers, paths, commands, quoted text such as error messages, and wording fixed by a convention (such as the English EARS keywords) stay as they are.

## Paths

The gsdd directory is `${CLAUDE_SKILL_DIR}`. Paths of gsdd files, in this file and in every gsdd file, are relative to that directory; paths starting with `docs/` are in the project's `docs/` directory. A subagent does not see this file: when dispatching one, give it the absolute path of the gsdd directory, this rule, and the language to use.

## Subcommands

Arguments: $ARGUMENTS

- No arguments: `status`.
- The first word is a subcommand name: that subcommand, with the remaining words as its arguments.
- Anything else: `req`, with the whole text as the request.

| Subcommand | For |
|------------|-----|
| `req` | Every new request or change: settling the intent and writing the requirements |
| `arch` | Choosing the technology of a new project, designing the architecture of the whole system, and changing it when capabilities need it |
| `design` | Designing a capability |
| `task` | Breaking a change into tasks |
| `impl` | Implementing the tasks |
| `verify` | Verifying a change and having the user accept it; checking that the documents and the code agree |
| `review` | Reviewing a change against the documents |
| `debug` | Finding the root cause of a failure, and fixing it when it lies in the code |
| `status` | Where things stand, and what to run next |

Read `<subcommand>/guide.md` and follow it. Read another subcommand's guide only when a guide asks for it.

This holds for the rest of the conversation: when the user later asks for another subcommand, by name or by describing it, read its guide and follow it. After a context summary, read again the guide of the subcommand in progress and the files it lists.
