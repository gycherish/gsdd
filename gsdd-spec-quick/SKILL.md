---
name: gsdd-spec-quick
description: Quick spec generation with interactive or automatic mode
allowed-tools: Read, Skill, Bash, Write, Glob, Agent
argument-hint: <feature-name> [--auto]
---

# Quick Spec Generator

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese. When a file follows a template, keep the template's headings and labels unchanged, without translating them.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

<instructions>
## CRITICAL: Automatic Mode Execution Rules

**If `--auto` flag is present in `$ARGUMENTS`, you are in AUTOMATIC MODE.**

In Automatic Mode:
- Execute ALL 3 phases in a continuous loop without stopping
- Display progress after each phase (e.g., "Phase 1/3 complete: requirements generated")
- IGNORE any "Next Step" messages from the phases (they are for standalone usage)
- After Phase 3, run the final sanity review before exiting
- Stop ONLY after the sanity review completes or if error occurs

---

## Core Task
Execute 3 spec phases (requirements, design, tasks) sequentially for a spec created by `/gsdd-discovery`. In automatic mode, execute all phases without stopping. In interactive mode, prompt user for approval between phases.

Before claiming quick generation is complete, run one lightweight sanity review over the generated requirements, design, and tasks. If the host supports fresh subagents, use one. Otherwise run the sanity review inline.

## Execution Steps

### Step 1: Parse Arguments and Check Spec

Parse `$ARGUMENTS`:
- If contains `--auto`: **Automatic Mode** (execute all 3 phases)
- Otherwise: **Interactive Mode** (prompt at each phase)
- Extract the feature name (remove `--auto` flag if present)

Example:
```
"user-profile-avatar-upload --auto" → mode=automatic, feature="user-profile-avatar-upload"
"user-profile-avatar-upload" → mode=interactive, feature="user-profile-avatar-upload"
```

Check the spec:
- `docs/specs/{feature-name}/brief.md` and `spec.json` must exist (created by `/gsdd-discovery`). If either is missing, stop without creating files and recommend running `/gsdd-discovery` first.
- If `phase` in `spec.json` is not `initialized`, the spec has already progressed: stop and suggest `/gsdd-spec-status {feature-name}` to see its next step.

Display mode banner and proceed to Step 2.

### Step 2: Execute Phase Loop

Execute these 3 phases in order:

---

#### Phase 1: Generate Requirements

Invoke the `gsdd-spec-requirements` skill with `{feature-name}`.

Wait for completion. IGNORE any "Next Step" message (it is for standalone usage).

**Output Progress**: "Phase 1/3 complete: Requirements generated"

**Automatic Mode**: IMMEDIATELY continue to Phase 2.

**Interactive Mode**: Prompt "Continue to design generation? (yes/no)"
- If "no": Stop, show current state
- If "yes": Continue to Phase 2

---

#### Phase 2: Generate Design

Invoke the `gsdd-spec-design` skill with `{feature-name} -y`. The `-y` flag auto-approves requirements.

Wait for completion. IGNORE any "Next Step" message.

**Output Progress**: "Phase 2/3 complete: Design generated"

**Automatic Mode**: IMMEDIATELY continue to Phase 3.

**Interactive Mode**: Prompt "Continue to tasks generation? (yes/no)"
- If "no": Stop, show current state
- If "yes": Continue to Phase 3

---

#### Phase 3: Generate Tasks

Invoke the `gsdd-spec-tasks` skill with `{feature-name} -y`.

Note: `-y` flag auto-approves requirements, design, and tasks.

Wait for completion.

**Output Progress**: "Phase 3/3 complete: Tasks generated"

#### Final Sanity Review

After Phase 3, run a lightweight sanity review before claiming completion.

- Review `requirements.md`, `design.md`, and `tasks.md` directly from disk. If `brief.md` exists, use it only as supporting context.
- Prefer a fresh review subagent when the host supports it. Pass only file paths and the review objective; the reviewer should read the generated files itself.
- Review focus:
  - Do requirements, design, and tasks tell a coherent story?
  - Are there obvious contradictions, missing prerequisites, or missing task coverage for required design work?
  - Are `_Depends:_`, `_Boundary:_`, and `(P)` markers plausible for implementation?
- If the review finds only task-plan-local issues, repair or update the generated `tasks.md` once, then re-run the sanity review.
- If the review finds a real requirements/design gap or contradiction, stop and report follow-up instead of claiming the quick spec is implementation-ready.

**All 3 phases plus sanity review complete.**

Output final completion summary (see Output Description section) and exit.

---

## Important Constraints

### Error Handling
- Any phase failure stops the workflow
- Display error and current state
- Suggest manual recovery command

</instructions>

## Output Description

### Mode Banners

**Interactive Mode**:
```
Quick Spec Generation (Interactive Mode)

You will be prompted at each phase.
Note: Skips design validation.
```

**Automatic Mode**:
```
Quick Spec Generation (Automatic Mode)

All phases execute automatically without prompts.
Note: Skips optional design review and user approval prompts. Internal review gates still run.
Final sanity review still runs.
```

### Intermediate Output

After each phase, show brief progress:
```
Requirements generated → Continuing to design...
Design generated → Continuing to tasks...
```

### Final Completion Summary

Provide output in the language specified in `spec.json`:

```
Quick Spec Generation Complete!

Generated Files:
- docs/specs/{feature}/requirements.md ({X} requirements)
- docs/specs/{feature}/design.md ({Y} components, {Z} endpoints)
- docs/specs/{feature}/tasks.md ({N} tasks)

Skipped: /gsdd-validate-design

Sanity review: PASSED | FOLLOW-UP REQUIRED

Next Steps:
1. Review generated specs (especially design.md)
2. Optional: `/gsdd-validate-design {feature}`
3. Start implementation: `/gsdd-impl {feature}`
```

## Safety & Fallback

### Error Scenarios

**Phase Execution Failed**:
- Stop workflow
- Show current state and completed phases
- Suggest: "Continue manually from `/gsdd-spec-{next-phase} {feature}`"

**Sanity Review Failed**:
- Stop workflow
- Report the exact contradiction, missing prerequisite, or task-plan issue
- Suggest targeted follow-up with `/gsdd-spec-design {feature}`, `/gsdd-spec-tasks {feature}`, or manual edits depending on the finding

**User Cancellation** (Interactive Mode):
- Stop gracefully
- Show completed phases
- Suggest manual continuation
