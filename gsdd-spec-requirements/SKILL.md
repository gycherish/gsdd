---
name: gsdd-spec-requirements
description: Generate EARS-format requirements from the feature brief and steering context. Use when generating requirements for a spec created by gsdd-discovery.
allowed-tools: Read, Write, Edit, Glob, Grep, Agent, WebSearch, WebFetch, AskUserQuestion
metadata:
  shared-rules: "ears-format.md, requirements-review-gate.md"
---

# gsdd-spec-requirements Skill

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese. When a file follows a template, keep the template's headings and labels unchanged, without translating them.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

## Core Mission
- **Success Criteria**:
  - Create complete requirements document aligned with steering context
  - Follow the project's EARS patterns and constraints for all acceptance criteria
  - Focus on core functionality without implementation details
  - Update metadata to track generation status

## Execution Steps

### Step 1: Gather Context

First check that `docs/specs/{feature}/brief.md` and `spec.json` exist. If either is missing, stop: do not create them or any other file (they are created only by `gsdd-discovery`), and recommend running `/gsdd-discovery` first.

Reuse steering/spec context already available from conversation; load missing context below:
- Read `docs/specs/{feature}/spec.json` for language and metadata
- Read `docs/specs/{feature}/brief.md`: the input for this phase (problem, current state, desired outcome, approach, scope decisions, boundary candidates)
- Core steering context: `product.md`, `tech.md`, `structure.md`
- Additional steering files only when directly relevant to feature scope, user personas, business/domain rules, compliance/security constraints, operational constraints, or existing product boundaries

### Step 2: Read Guidelines
- Read `rules/ears-format.md` from this skill's directory for EARS syntax rules
- Read `rules/requirements-review-gate.md` from this skill's directory for pre-write review criteria
- Read `templates/specs/requirements.md` (in this skill's directory) for document structure

#### Parallel Research (subagent dispatch)

The following research areas are independent. Decide the optimal decomposition based on project complexity -- split, merge, add, or skip subagents as needed.

**Delegate to subagent via Agent tool** (keeps exploration out of main context):
- **Codebase hints** (brownfield projects): Dispatch a subagent to explore existing implementations that inform requirement scope. Example prompt: "Explore this codebase for existing features related to [feature area]. Summarize: (1) what already exists, (2) relevant interfaces/APIs, (3) patterns that new requirements should align with. Return a summary under 150 lines."
- **Domain research** (when external knowledge is needed): Dispatch a subagent for WebSearch/WebFetch to research domain-specific requirements, standards, or best practices. Return a concise findings summary.
- **Additional steering**: If many steering files exist, dispatch a subagent to scan them and return only the sections relevant to this feature.

For greenfield projects with minimal codebase, skip subagent dispatch and load context directly.

After all research completes, synthesize findings in main context before generating requirements.

### Step 3: Generate Requirements Draft
- Create initial requirements draft based on the brief
- Group related functionality into logical requirement areas
- Apply EARS format to all acceptance criteria
- Use language specified in spec.json
- Keep this as a draft until the review gate passes; do not write `requirements.md` yet

### Step 4: Review Requirements Draft
- Run the `Requirements Review Gate` from `rules/requirements-review-gate.md`
- Review coverage, EARS compliance, and ambiguity before finalizing
- If issues are local to the draft, repair the requirements and review again
- Keep the review bounded to at most 2 repair passes
- If the draft exposes a real scope ambiguity or contradiction, stop and ask the user to clarify instead of writing guessed requirements

### Step 5: Finalize and Update Metadata
- Write `docs/specs/{feature}/requirements.md` only after the requirements review gate passes
- Set `phase: "requirements-generated"`
- Set `approvals.requirements.generated: true`
- Update `updated_at` timestamp

## Important Constraints

### Requirements Scope: WHAT, not HOW
Requirements describe user-observable behavior, not implementation. Use this to decide what belongs here vs. in design:

**Ask the user about (requirements scope):**
- Functional scope — what the feature must do
- User-observable behavior — "when X happens, what should the user see/experience?"
- Business rules and edge cases — limits, error conditions, special cases
- Non-functional requirements visible to users — response time expectations, availability, security level

**Do not ask about (design scope — defer to design phase):**
- Technology stack choices (database, framework, language)
- Architecture patterns (microservices, monolith, event-driven)
- API design, data models, internal component structure
- How to achieve non-functional requirements (caching strategy, scaling approach)
- Internal ownership mapping, component seams, or implementation boundaries that belong in design

**Litmus test**: If an EARS acceptance criterion can be written without mentioning any technology, it belongs in requirements. If it requires a technology choice, it belongs in design.

### Other Constraints
- Each requirement must be testable and unambiguous. If the brief leaves room for multiple interpretations on scope, behavior, or boundary conditions, ask the user to clarify before generating that requirement. Ask as many questions as needed; do not generate requirements that contain your own assumptions.
- Choose appropriate subject for EARS statements (system/service name for software)
- Requirement headings use `### Requirement N: <area name>` with a numeric `N` (1, 2, 3); do not use alphabetic IDs like "Requirement A".
- Number acceptance criteria `N.M`, where `N` is the requirement number and `M` is the criterion's position under it (1.1, 1.2, 2.1). Later phases reference acceptance criteria by these IDs.

## Output Description
Provide output in the language specified in spec.json with:

1. **Generated Requirements Summary**: Brief overview of major requirement areas (3-5 bullets)
2. **Document Status**: Confirm requirements.md updated and spec.json metadata updated
3. **Review Gate**: Confirm the requirements review gate passed
4. **Next Steps**: Guide user on how to proceed (approve and continue, or modify)

**Format Requirements**:
- Use Markdown headings for clarity
- Include file paths in code blocks
- Keep summary concise (under 300 words)

## Safety & Fallback

### Error Scenarios
- **Missing Brief or spec.json**: If `docs/specs/{feature}/brief.md` or `spec.json` does not exist, stop without creating files and recommend running `/gsdd-discovery` first
- **Template Missing**: If template files don't exist, use inline fallback structure with warning
- **Incomplete Requirements**: After generation, explicitly ask user if requirements cover all expected functionality
- **Steering Directory Empty**: Warn user that project context is missing and may affect requirement quality
- **Non-numeric Requirement Headings**: If existing headings do not include a leading numeric ID (for example, they use "Requirement A"), normalize them to numeric IDs and keep that mapping consistent (never mix numeric and alphabetic labels).

### Next Phase: Design Generation

**If Requirements Approved**:
- **Optional Gap Analysis** (for existing codebases):
  - Run `/gsdd-validate-gap {feature}` to analyze implementation gap
  - Recommended for brownfield projects; skip for greenfield
- Run `/gsdd-spec-design {feature}` to proceed to design phase
- Or `/gsdd-spec-design {feature} -y` to auto-approve requirements and proceed directly

**If Modifications Needed**:
- Provide feedback and re-run `/gsdd-spec-requirements {feature}`
