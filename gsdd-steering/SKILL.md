---
name: gsdd-steering
description: Maintain docs/steering/ as persistent project memory (record/bootstrap/sync). Use when initializing or updating steering documents, or when another gsdd skill needs to record confirmed project-level decisions.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
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
- Read `rules/steering-principles.md` from this skill's directory for steering principles

## Scenario Detection

Check `docs/steering/` status:

**Record Mode**: Invoked with a set of confirmed project-level decisions (from another gsdd skill such as `gsdd-discovery`, or from the user)
**Bootstrap Mode**: No decisions supplied, and steering is empty OR missing core files (product.md, tech.md, structure.md)
**Sync Mode**: No decisions supplied, and all core files exist

---

## Record Flow

Steering grows with the requirements: in a new project, product, technology, and structure decisions are made during requirements analysis, and this mode records them as they are confirmed.

1. Take the decisions supplied by the caller. Record only decisions marked as confirmed; never infer or add decisions of your own.
2. Place each decision in the file and section of the steering templates in `templates/steering/` (in this skill's directory) that it matches. The templates are the only definition of the steering format: do not add sections they do not define, except the Open Questions section in step 5. If a decision fits no section, do not force it in: report it to the caller as an unplaced decision.
3. If a target file does not exist, create it from the template in `templates/steering/` (in this skill's directory) and fill only the sections covered by confirmed decisions; leave other sections as empty placeholders. Partial steering is expected at this stage.
4. If the file exists, add to it without replacing existing content. If a decision contradicts existing content, do not overwrite it: report the conflict to the caller.
5. Record items that are known to be undecided in an "Open Questions" section at the end of the relevant file, not as conclusions.
6. Apply `rules/steering-principles.md` (patterns over lists, no secrets).
7. Return a short summary: files created or updated, sections written, open questions added, conflicts found, unplaced decisions.

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
5. Generate steering files (follow templates)
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
4. Propose updates (additive, preserve user content)
5. Report: Updates, warnings, recommendations

**Update Philosophy**: Add, don't replace. Preserve user sections.

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

Chat summary only (files updated directly).

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

### Bootstrap:
```
Steering Created

## Generated:
- product.md: [Brief description]
- tech.md: [Key stack]
- structure.md: [Organization]

Review and approve as Source of Truth.
```

### Sync:
```
Steering Updated

## Changes:
- tech.md: Core library upgraded to a new major version
- structure.md: Added API pattern

## Code Drift:
- Components not following import conventions

## Recommendations:
- Consider api-standards.md
```

## Examples

### Record
**Input**: Called by `gsdd-discovery` in a new project with confirmed decisions: product purpose and value, chosen architecture and its rationale
**Output**: product.md and tech.md created with only the matching template sections filled; structure.md not created because no structural decision was made yet

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
- Templates and principles are external for customization
- Focus on patterns, not catalogs
- "Golden Rule": New code following patterns shouldn't require steering updates
- gsdd skill files under `.claude/skills/` (including their `templates/` and `rules/`) should NOT be documented in steering files (they are workflow metadata, not project knowledge)
