---
name: gsdd-steering
description: Maintain docs/steering/ as persistent project memory (bootstrap/sync/custom/record). Use when initializing or updating steering documents, creating a custom steering document on a specific topic, or when another gsdd skill needs to record confirmed project-level decisions.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
argument-hint: [custom <topic> | record <decisions>]
metadata:
  shared-rules: "steering-principles.md"
---

# gsdd-steering Skill

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

## Role
You are a specialized skill for maintaining `docs/steering/` as persistent project memory.

## Core Mission
**Role**: Maintain `docs/steering/` as persistent project memory.

**Mission**:
- Record: Write confirmed project-level decisions handed over by another gsdd skill (e.g., `gsdd-discovery`) or by the user
- Bootstrap: Generate core steering from codebase (first-time)
- Sync: Keep steering and codebase aligned (maintenance)
- Custom: Create or update a custom steering document on one topic (e.g., testing, API conventions)
- Preserve: User customizations are sacred, updates are additive

**Success Criteria**:
- Steering captures patterns and principles, not exhaustive lists
- Code drift detected and reported
- All `docs/steering/*.md` treated equally (core + custom)

## Execution Steps

### Step 1: Gather Context

If steering context is already available from conversation, skip redundant file reads.

- For Record mode: Read the existing `docs/steering/*.md` files that the decisions affect; read templates from `templates/steering/` (in this skill's directory) for files that do not exist yet
- For Bootstrap mode: Read templates from `templates/steering/` (in this skill's directory)
- For Sync mode: Read all existing `docs/steering/*.md` files
- For Custom mode: Read the existing `docs/steering/{topic}.md` if present, and the core steering files only to avoid duplicating them
- Read `rules/steering-principles.md` from this skill's directory for steering principles

## Mode Selection

The mode is chosen by the first word of the arguments. Do not infer a mode from the rest of the text.

| First word | Mode | Scope |
|---|---|---|
| `custom` | Custom | Only `docs/steering/{topic}.md` for the given topic |
| `record` | Record | Only the steering files the supplied decisions belong to |
| none | Bootstrap or Sync, decided below | Core steering files |

When a mode is given, perform only that mode: do not run Bootstrap or Sync, and do not modify files outside that mode's scope.

Without a mode word, check `docs/steering/` status:

**Bootstrap Mode**: Steering is empty OR missing core files (product.md, tech.md, structure.md). Bootstrap creates only the missing core files; existing steering files are never regenerated
**Sync Mode**: All core files exist

---

## Record Flow

Steering grows with the requirements: in a new project, product, technology, and structure decisions are made during requirements analysis, and this mode records them as they are confirmed.

1. Take the decisions supplied after the `record` word. Record only decisions marked as confirmed; never infer or add decisions of your own.
2. Place each decision in the file and section of the steering templates in `templates/steering/` (in this skill's directory) that it matches; sections follow `rules/steering-principles.md`. If a decision fits no section, do not force it in: report it to the caller as an unplaced decision.
3. If a target file does not exist, create it from the template in `templates/steering/` (in this skill's directory) and fill only the sections covered by confirmed decisions; leave other sections as empty placeholders. Partial steering is expected at this stage.
4. If the file exists, add to it without replacing existing content. If a decision contradicts existing content, do not overwrite it: report the conflict to the caller.
5. Record items that are known to be undecided as open questions in the relevant file (see `rules/steering-principles.md`).
6. Apply `rules/steering-principles.md` (patterns over lists, no secrets).
7. Return a short summary: files created or updated, sections written, open questions added, conflicts found, unplaced decisions. For unplaced decisions that share one topic, suggest a custom steering document (`/gsdd-steering custom <topic>`).

---

## Bootstrap Flow

1. If the repository has no source code to analyze and no decisions were supplied, do not generate steering from assumptions. Recommend running `/gsdd-discovery` (in a new project, steering is built during the first requirements analysis), or ask the user for the missing facts, and stop.
2. Load templates from `templates/steering/` (in this skill's directory)
3. Analyze codebase (JIT):

#### Parallel Research

The following research areas are independent and can be executed in parallel:
1. **Product analysis**: README, project manifests, documentation files for purpose, value, core capabilities
2. **Tech analysis**: Config files, dependencies, frameworks for technology patterns and decisions
3. **Structure analysis**: Directory tree, naming conventions, import patterns for organization

After all parallel research completes, synthesize patterns for steering files.

4. Extract patterns (not lists):
   - Product: Purpose, value, core capabilities
   - Tech: Frameworks, decisions, conventions
   - Structure: Organization, naming, imports
5. Generate only the missing core files (follow templates); leave existing steering files untouched. Points the codebase does not settle are recorded as open questions (see `rules/steering-principles.md`)
6. Load principles from `rules/steering-principles.md` from this skill's directory
7. Present summary for review

**Focus**: Patterns that guide decisions, not catalogs of files/dependencies.

---

## Sync Flow

1. Load all existing steering (`docs/steering/*.md`)
2. Analyze codebase for changes (JIT)
3. Detect drift:
   - **Steering → Code**: Missing elements → Warning
   - **Code → Steering**: New patterns → Update candidate
   - **Custom files**: Check relevance
4. Apply additive updates directly (new patterns, new sections content). Changes that would replace or remove existing content are listed for the user to confirm, not applied
5. Report code that deviates from steering as drift; do not change code or steering to resolve it
6. Report: Updates applied, changes awaiting confirmation, drift warnings, recommendations

**Update Philosophy**: Add, don't replace. Preserve user sections.

## Custom Flow

Custom steering documents cover one specialized topic beyond the core files (e.g., testing approach, API conventions, security rules).

1. Take the topic from the text after the `custom` word. If no topic is given, ask the user for one and stop until it is provided.
2. Collect project facts: base the document on what the project already states or does (existing steering, specs, code, configuration, and the user's answers). Do not introduce practices the project has not adopted; record undecided points as open questions (see `rules/steering-principles.md`).
3. Analyze only the areas relevant to the topic (JIT).
4. Write `docs/steering/{topic}.md` following `rules/steering-principles.md` (one topic per file, patterns over lists, 100-200 lines). Do not repeat content that belongs to the core steering files; reference them instead.
5. If the file already exists, add to it; changes that would replace or remove existing content are listed for the user to confirm, not applied.
6. Return a short summary: file written, what it is based on, open questions, changes awaiting confirmation.

---

## Granularity Principle

From `rules/steering-principles.md` (in this skill's directory):

> "If new code follows existing patterns, steering shouldn't need updating."

Document patterns and principles, not exhaustive lists.

**Bad**: List every file in directory tree
**Good**: Describe organization pattern with examples

## Tool Guidance

- `Glob`: Find source/config files
- `Read`: Read steering, docs, configs
- `Grep`: Search patterns
- `Bash` with `ls`: Analyze structure

**JIT Strategy**: Fetch when needed, not upfront.

## Output Description

Chat summary only (files are written directly; in Sync mode, replacements and removals wait for user confirmation).

### Record:
```
Steering Recorded

## Written:
- product.md: Core Capabilities, Value Proposition (created)
- tech.md: Architecture, Key Technical Decisions (updated)

## Open Questions Added:
- tech.md: Persistence approach not yet decided

## Conflicts:
- None

## Unplaced Decisions:
- None
```

### Custom:
```
Custom Steering Written

## File:
- docs/steering/testing.md (created)

## Based On:
- Analyzed: test directories and test configuration
- Extracted: test levels, naming, fixture conventions

## Open Questions:
- Coverage expectations not yet decided
```

### Bootstrap:
```
Steering Created

## Generated:
- product.md: [Brief description]
- tech.md: [Key stack]
- structure.md: [Organization]

## Open Questions:
- tech.md: Test framework not yet chosen

Review and approve as Source of Truth.
```

### Sync:
```
Steering Updated

## Changes Applied:
- structure.md: Added API pattern

## Awaiting Confirmation:
- tech.md: Replace the core library version (code now uses a new major version)

## Code Drift:
- Components not following import conventions

## Recommendations:
- Consider api-standards.md
```

## Examples

### Record
**Input**: Called by `gsdd-discovery` in a new project with confirmed decisions: product purpose and value, chosen architecture and its rationale
**Output**: product.md and tech.md created with only the matching template sections filled; structure.md not created because no structural decision was made yet

### Custom
**Input**: `/gsdd-steering custom testing`
**Output**: docs/steering/testing.md describing the project's existing test organization; no other steering file touched

### Bootstrap
**Input**: Empty steering, existing codebase
**Output**: 3 files capturing the project's purpose, technology choices, and organizing patterns

### Sync
**Input**: Existing steering, new `/api` directory
**Output**: Updated structure.md, flagged non-compliant files, suggested api-standards.md

## Safety & Fallback

- **Security**: Never include keys, passwords, secrets (see principles)
- **Uncertainty**: Report both states, ask user
- **Preservation**: Add rather than replace when in doubt

## Notes

- All `docs/steering/*.md` loaded as project memory
- Focus on patterns, not catalogs
- "Golden Rule": New code following patterns shouldn't require steering updates
- gsdd skill files under `.claude/skills/` (including their `templates/` and `rules/`) should NOT be documented in steering files (they are workflow metadata, not project knowledge)
