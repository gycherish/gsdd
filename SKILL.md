---
name: gsdd
description: Design documents for a new project. Researches the product, and writes its requirements, user interface, architecture, and detailed design before the code; implements from them, and keeps them true as the code changes.
argument-hint: "[req | ui | arch | design | impl] [arguments]"
disable-model-invocation: true
---

# gsdd

## Language

Think, reply to the user, and write every document, headings included, in Simplified Chinese, unless the user asks for another language, or the project's documents in `docs/` are written in another one; then use that one. This holds for every message to the user, the short notes between tool calls as well as the replies, whatever language the request, the gsdd files, or the tool output are written in. Code identifiers, paths, commands, and quoted text stay as they are.

## Paths

The gsdd directory is `${CLAUDE_SKILL_DIR}`. Paths of gsdd files, in this file and in every gsdd file, are relative to that directory; paths starting with `docs/` are in the project's `docs/` directory. A subagent does not see this file: when dispatching one, give it the absolute path of the gsdd directory, this rule, and the language to use, and tell it to do its work itself, dispatching no subagents of its own.

## Commands

Arguments: $ARGUMENTS

- No arguments: report where the documents stand (below).
- The first word is a command name: that command, with the remaining words as its arguments.
- Anything else: `req`, with the whole text as the request.

| Command | For |
|---------|-----|
| `req` | Researching the product, writing its requirements, and choosing the features of each version; every request that adds or changes features |
| `ui` | Designing the user interface: the screens, the flows, the visual style, and a prototype |
| `arch` | Choosing the technology and designing the architecture of the version being built |
| `design` | Designing how a module or a feature is implemented, for those that need it |
| `impl` | Implementing the product from its documents |

Read `<command>/guide.md` and follow it. Read another command's guide only when a guide asks for it.

This holds for the rest of the conversation: when the user later asks for another command, by name or by describing it, read its guide and follow it. After a context summary, read again the guide of the command in progress and the files it lists.

## Order

The documents are written in the order `req` → `ui` → `arch`; a product without a user interface has no `ui`. Then the product is built piece by piece: `design` writes the designs the next piece needs, and `impl` builds it. When the user confirms the result of a step, go on at once to the next step the work needs, as the step's guide says. Each command can also be run on its own.

## Where Things Stand

With no arguments, read `docs/README.md` and the documents it lists, and report: the documents that exist; the version being built and its features; the parts that have a design; and the command to run next. In a project without `docs/product/requirements.md`, suggest describing the product to build with `/gsdd <what to build>`.
