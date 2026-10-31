---
name: gsdd-spec-init
description: Initialize a new specification with detailed project description
allowed-tools: Bash, Read, Write, Glob, AskUserQuestion
argument-hint: <feature-name | project-description>
---

# Spec Initialization

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

<instructions>
## Core Task
Determine the feature name from $ARGUMENTS and initialize the specification structure (spec.json and requirements.md).

## Execution Steps
1. **Determine Feature Name**:
   - If $ARGUMENTS is exactly the name of an existing `docs/specs/{name}/` directory that contains `brief.md` (created by `/gsdd-discovery`), use that name.
   - Otherwise generate a name from the description: lowercase English words joined by hyphens, 2-4 words (e.g., "User profile with avatar upload" → `user-profile-avatar-upload`). If no name is clearly better than the others, propose 2-3 options and ask the user to choose.
2. **Check for Brief**: If `docs/specs/{feature-name}/brief.md` exists, read it. The brief contains problem, approach, scope, and constraints from the discovery session; skip clarification questions that the brief already answers.
3. **Clarify Intent**: The project description must cover three elements: (a) who has the problem, (b) current situation, (c) what should change. If the brief or $ARGUMENTS covers these, skip to step 4. Otherwise, ask the user to clarify before proceeding. Ask as many questions as needed; do not fill in gaps with your own assumptions.
4. **Check Existing Spec**: If `docs/specs/{feature-name}/spec.json` already exists, stop without creating, overwriting, or renaming anything. Report that the spec exists and its current `phase`, and ask the user whether to continue that spec (suggest `/gsdd-spec-status {feature-name}` to see its next step) or to use a different name. If the directory exists with only `brief.md`, use that directory.
5. **Create Directory**: `docs/specs/{feature-name}/` (skip if it already exists)
6. **Initialize Files Using Templates**:
   - Read `templates/specs/init.json` (in this skill's directory)
   - Read `templates/specs/requirements-init.md` (in this skill's directory)
   - Replace placeholders:
     - `{{FEATURE_NAME}}` → the feature name from step 1
     - `{{TIMESTAMP}}` → current ISO 8601 timestamp
     - `{{PROJECT_DESCRIPTION}}` → if `brief.md` exists: a 1-2 sentence summary of the problem and the desired outcome, followed by a reference to `brief.md` in the same directory for details; do not copy the brief. Otherwise: the description from $ARGUMENTS together with the user's answers from step 3.
     - `{{LANG_CODE}}` → language code: the language the user explicitly specifies; if none is specified, `zh`. Do not infer it from the language of the user's input.
   - Write the project description in the language given by `{{LANG_CODE}}`
   - Write `spec.json` and `requirements.md` to the spec directory

## Important Constraints
- Do NOT generate requirements, design, or tasks. This skill only creates spec.json and requirements.md.
</instructions>

## Output Description
Provide output in the language specified in `spec.json` with the following structure:

1. **Feature Name**: `feature-name`, and whether it comes from an existing brief directory or was generated (with a 1-sentence rationale when generated)
2. **Project Summary**: Brief summary (1 sentence)
3. **Created Files**: Bullet list with full paths
4. **Next Step**: Command block showing `/gsdd-spec-requirements <feature-name>`

**Format Requirements**:
- Use Markdown headings (##, ###)
- Wrap commands in code blocks
- Keep total output concise (under 250 words)
- Use clear, professional language per the `language` field in `spec.json`

## Safety & Fallback
- **Template Missing**: If template files don't exist in `templates/specs/` (in this skill's directory), report error with specific missing file path and suggest checking repository setup
- **Write Failure**: Report error with specific path and suggest checking permissions or disk space
