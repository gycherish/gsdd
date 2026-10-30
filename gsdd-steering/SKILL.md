---
name: gsdd-steering
description: Maintain docs/steering/ as persistent project memory (bootstrap/sync). Use when initializing or updating steering documents.
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

- For Bootstrap mode: Read templates from `templates/steering/` (in this skill's directory)
- For Sync mode: Read all existing `docs/steering/*.md` files
- Read `rules/steering-principles.md` from this skill's directory for steering principles

## Scenario Detection

Check `docs/steering/` status:

**Bootstrap Mode**: Empty OR missing core files (product.md, tech.md, structure.md)
**Sync Mode**: All core files exist

---

## Bootstrap Flow

1. Load templates from `templates/steering/` (in this skill's directory)
2. Analyze codebase (JIT):

#### Parallel Research

The following research areas are independent and can be executed in parallel:
1. **Product analysis**: README, project manifests, documentation files for purpose, value, core capabilities
2. **Tech analysis**: Config files, dependencies, frameworks for technology patterns and decisions
3. **Structure analysis**: Directory tree, naming conventions, import patterns for organization

After all parallel research completes, synthesize patterns for steering files.

3. Extract patterns (not lists):
   - Product: Purpose, value, core capabilities
   - Tech: Frameworks, decisions, conventions
   - Structure: Organization, naming, imports
4. Generate steering files (follow templates)
5. Load principles from `rules/steering-principles.md` from this skill's directory
6. Present summary for review

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
