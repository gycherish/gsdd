---
name: gsdd-steering-custom
description: Create custom steering documents for specialized project contexts. Use when creating domain-specific steering files.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
metadata:
  shared-rules: "steering-principles.md"
---

# gsdd-steering-custom Skill

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

## Role
You are a specialized skill for creating custom steering documents beyond core files (product, tech, structure).

## Core Mission
**Role**: Create specialized steering documents beyond core files (product, tech, structure).

**Mission**: Help users create domain-specific project memory for specialized areas.

**Success Criteria**:
- Custom steering captures specialized patterns
- Follows same granularity principles as core steering
- Provides clear value for specific domain

## Execution Steps

### Step 1: Gather Context

If steering context is already available from conversation, skip redundant file reads.
Otherwise:
- Read `rules/steering-principles.md` from this skill's directory for steering principles

## Workflow

1. **Ask user** for custom steering needs:
   - Domain/topic (e.g., "API standards", "testing approach")
   - Specific requirements or patterns to document

2. **Collect project facts**:
   - Base the document on what the project already states or does: existing steering, specs, code, configuration, and the user's answers
   - Do not introduce practices the project has not adopted; record undecided points as open questions for the user

3. **Analyze codebase** (JIT) for relevant patterns:

#### Parallel Research

The following research areas are independent and can be executed in parallel:
1. **Principles**: Load steering-principles.md
2. **Domain patterns**: Analyze codebase for domain-specific patterns using Glob/Grep/Read

After all parallel research completes, synthesize findings for steering document.

4. **Generate custom steering**:
   - Apply principles from `rules/steering-principles.md` from this skill's directory
   - Focus on patterns, not exhaustive lists
   - Keep to 100-200 lines (2-3 minute read)

5. **Create file** in `docs/steering/{name}.md`

## Steering Principles

From `rules/steering-principles.md` (in this skill's directory):

- **Patterns over lists**: Document patterns, not every file/component
- **Single domain**: One topic per file
- **Concrete examples**: Show patterns with code
- **Maintainable size**: 100-200 lines typical
- **Security first**: Never include secrets or sensitive data

## Tool Guidance

- **Read**: Analyze existing code, configuration, and documents
- **Glob**: Find related files for pattern analysis
- **Grep**: Search for specific patterns
- **Bash** with `ls`: Understand relevant structure

**JIT Strategy**: Analyze only the areas relevant to the requested topic.

## Output Description

Chat summary with file location (file created directly).

```
Custom Steering Created

## Created:
- docs/steering/api-standards.md

## Based On:
- Analyzed: src/api/ directory patterns
- Extracted: endpoint naming, error format

## Content:
- Endpoint naming patterns
- Request/response format
- Error handling conventions
- Authentication approach

Review and customize as needed.
```

## Examples

### Success: API Standards
**Input**: "Create API standards steering"
**Action**: Analyze src/api/, extract existing patterns
**Output**: api-standards.md with the project's own API conventions

### Success: Testing Strategy
**Input**: "Document our testing approach"
**Action**: Analyze test files, extract existing patterns
**Output**: testing.md with test organization and mocking strategies

## Safety & Fallback

- **No existing patterns**: Do not invent conventions; ask the user and record undecided points as open questions
- **Security**: Never include secrets (load principles)
- **Validation**: Ensure doesn't duplicate core steering content

## Notes

- Follow same granularity principles as core steering
- All steering files loaded as project memory
- Custom files equally important as core files
- Avoid documenting agent-specific tooling directories (e.g. `.cursor/`, `.gemini/`, `.claude/`)
- Light references to `docs/specs/` and `docs/steering/` are acceptable; avoid documenting the SDD workflow files under `.claude/skills/`
