---
name: gsdd-spec-design
description: Generate comprehensive technical design translating requirements (WHAT) into architecture (HOW) with discovery process. Use when creating architecture from requirements.
allowed-tools: Read, Write, Edit, Grep, Glob, WebSearch, WebFetch, Agent, Skill
argument-hint: <feature-name> [-y]
metadata:
  shared-rules: "design-principles.md, design-discovery-full.md, design-discovery-light.md, design-synthesis.md, design-review-gate.md"
---

# gsdd-spec-design Skill

> **Language**: Think, respond, and write files in the language set in `spec.json` (`language` field); if none is set, use Simplified Chinese. When a file follows a template, keep the template's headings and labels unchanged, without translating them.
>
> **Format precedence**: Document structure, numbering, EARS syntax, and report formats come only from gsdd skills and their bundled `templates/` and `rules/`; other skills may inform content but must not change these formats.

## Core Mission
- **Success Criteria**:
  - All requirements mapped to technical components with clear interfaces
  - The design makes responsibility boundaries explicit enough to guide task generation and review
  - Appropriate architecture discovery and research completed
  - Design aligns with steering context and existing patterns
  - Visual diagrams included for complex architectures

## Execution Steps

### Step 1: Gather Context

Reuse steering/spec context already available from conversation; load missing context below:
- `docs/specs/{feature}/spec.json`, `requirements.md`, `design.md` (if exists)
- `docs/specs/{feature}/research.md` (if exists)
- Core steering context: `product.md`, `tech.md`, `structure.md`
- Additional steering files only when directly relevant to requirement coverage, architecture boundaries, integrations, runtime prerequisites, security/performance constraints, or team conventions that affect implementation readiness
- `templates/specs/design.md` (in this skill's directory) for document structure
- Read `rules/design-principles.md` from this skill's directory for design principles
- `templates/specs/research.md` (in this skill's directory) for discovery log structure

**Validate requirements approval**:
- If auto-approve flag is true: Auto-approve requirements in spec.json
- Otherwise: Verify approval status (stop if unapproved, see Safety & Fallback)

### Step 2: Discovery & Analysis

**Critical: This phase ensures design is based on complete, accurate information.**

1. **Classify Feature Type**:
   - **New Feature** (greenfield) → Full discovery required
   - **Extension** (existing system) → Integration-focused discovery
   - **Simple Addition** (CRUD/UI) → Minimal or no discovery
   - **Complex Integration** → Comprehensive analysis required

2. **Execute Appropriate Discovery Process**:

   **For Complex/New Features**:
   - Read and execute `rules/design-discovery-full.md` from this skill's directory
   - Research external libraries, services, or protocols the design introduces with WebSearch/WebFetch, following the research guidelines in that file

   **For Extensions**:
   - Read and execute `rules/design-discovery-light.md` from this skill's directory
   - Focus on integration points, existing patterns, compatibility, and the requirement-to-code map
   - Use Grep to analyze existing codebase patterns

   **For Simple Additions**:
   - Skip formal discovery, quick pattern check only

#### Parallel Research (subagent dispatch)

The following research areas are independent and can be dispatched as **subagents** via the Agent tool. The agent should decide the optimal decomposition based on feature complexity — split, merge, add, or skip subagents as needed. Each subagent returns a **findings summary** (not raw data) to keep the main context clean for synthesis.

**Typical research areas** (adjust as appropriate):
- **Codebase analysis**: Existing architecture patterns, integration points, code conventions (using Grep/Glob)
- **External research**: Dependencies, APIs, latest best practices (using WebSearch/WebFetch)
- **Context loading** (usually main context): Steering files, design principles, discovery rules, templates

For simple additions, skip subagent dispatch entirely and do a quick pattern check in main context.

After all findings return, synthesize in main context before proceeding.

3. **Retain Discovery Findings for Step 3**:
   - External API contracts and constraints
   - Technology decisions with rationale
   - Existing patterns to follow or extend
   - Integration points and dependencies
   - Identified risks and mitigation strategies
   - Boundary candidates, responsibilities owned elsewhere, and contracts other specs consume

4. **Persist Findings to Research Log**:
   - Create or update `docs/specs/{feature}/research.md` using the shared template
   - Summarize discovery scope and key findings
   - Record investigations with sources and implications
   - Document architecture pattern evaluation, design decisions, and risks
   - Use the language specified in spec.json when writing or updating `research.md`

### Step 3: Synthesis

**Apply design synthesis to discovery findings before writing.**

- Read and apply `rules/design-synthesis.md` from this skill's directory
- This step requires the full picture from discovery findings — execute in main context, not in a subagent
- Record synthesis outcomes (generalizations found, build-vs-adopt decisions, simplifications) in `research.md`

### Step 4: Generate Design Draft

1. **Generate Design Draft**:
   - **Follow `templates/specs/design.md` (in this skill's directory)**: keep its fixed sections (Overview, Components, Change Plan, Verification, Risks and Open Questions), and choose the body's sections for this feature as described in "Choosing the Body" in `rules/design-principles.md`
   - **Boundary-first requirement**: Before writing the body in detail, settle the boundary: what each component owns, what it depends on, which related responsibilities belong to other specs or modules, and which contracts other specs consume.
   - **Integrate all discovery findings and synthesis outcomes**: Use researched information (APIs, patterns, technologies) and synthesis decisions (generalizations, build-vs-adopt, simplifications) throughout component definitions, architecture decisions, and integration points
   - **Decisions**: State every decision implementers must follow, with its rationale and the main alternative rejected; mark the ones that hold for the whole project with (project-level). Step 6 records those into steering.
   - **Change Plan** (required): Analyze the codebase to determine where the change lands, and fill the Change Plan at the granularity the template describes. It drives task `_Boundary:_` annotations and implementation task briefs; a vague change plan produces vague implementations.
   - **Verification**: For every acceptance criterion, state what is exercised and what result proves it, referencing specific components and behaviors. Avoid vague entries like "test login works".
   - If existing design.md found in Step 1, use it as reference context (merge mode)
   - Apply the rules in `rules/design-principles.md`
   - Use language specified in spec.json
   - Keep this as a draft until the review gate passes; do not write `design.md` yet

### Step 5: Review Design Draft

- Read and apply `rules/design-review-gate.md` from this skill's directory
- Verify requirements coverage, architecture readiness, and implementation executability before finalizing the design
- If issues are local to the draft, repair the design and review again
- Keep the review bounded to at most 2 repair passes
- If the draft exposes a real requirements/design gap, stop and return to requirements clarification instead of papering over it in `design.md`

### Step 6: Finalize Design Document

1. **Write Final Design**:
   - Write `docs/specs/{feature}/design.md` only after the design review gate passes
   - Write research.md with discovery findings and synthesis outcomes (if not already written)

2. **Record Project-Level Decisions**:
   - If this design settles project-level decisions that steering does not record yet (the decisions marked (project-level): decisions that hold for the whole project rather than this feature, such as adopting a new library, a directory or naming convention, or a cross-cutting technical rule), invoke the `gsdd-steering` skill with `record` as the first word of its arguments, followed by those decisions with their rationale. Do not write steering files directly.
   - If the caller asked you not to record into steering (as `gsdd-spec-batch` does), list these decisions in the output instead.
   - If `gsdd-steering` reports a conflict with existing steering, report it to the user instead of overriding steering.
   - Skip this step when the design makes no new project-level decision.

3. **Update Metadata** in spec.json:

   - Set `phase: "design-generated"`
   - Set `approvals.design.generated: true, approved: false`
   - Set `approvals.requirements.approved: true`
   - Update `updated_at` timestamp

## Critical Constraints
 - **Explicit Interfaces**:
   - Define explicit types and interfaces at component boundaries, following the language and conventions recorded in steering and this spec.
   - Document public interfaces and contracts clearly to ensure cross-component consistency.
- **Requirement IDs**: Use numeric requirement IDs only (e.g. "1.1", "1.2", "3.1", "3.3") exactly as defined in requirements.md. Do not invent new IDs or use alphabetic labels.

## Output Description

**Command execution output** (separate from design.md content):

Provide brief summary in the language specified in spec.json:

1. **Status**: Confirm design document generated at `docs/specs/{feature}/design.md`
2. **Discovery Type**: Which discovery process was executed (full/light/minimal)
3. **Key Findings**: 2-3 critical insights from discovery that shaped the design
4. **Review Gate**: Confirm the design review gate passed
5. **Next Action**: Approval workflow guidance (see Safety & Fallback)
6. **Research Log**: Confirm `research.md` updated with latest decisions
7. **Steering**: Project-level decisions recorded into steering (or listed for the caller), or none

**Format**: Concise Markdown (under 200 words) - this is the command output, NOT the design document itself

**Note**: The actual design document follows `templates/specs/design.md` (in this skill's directory) structure.

## Safety & Fallback

### Error Scenarios

**Requirements Not Approved**:
- **Stop Execution**: Cannot proceed without approved requirements
- **User Message**: "Requirements not yet approved. Approval required before design generation."
- **Suggested Action**: "Run `/gsdd-spec-design {feature} -y` to auto-approve requirements and proceed"

**Missing Requirements**:
- **Stop Execution**: Requirements document must exist
- **User Message**: "No requirements.md found at `docs/specs/{feature}/requirements.md`"
- **Suggested Action**: "Run `/gsdd-spec-requirements {feature}` to generate requirements first"

**Template Missing**:
- **User Message**: "Template file missing at `templates/specs/design.md` (in this skill's directory)"
- **Suggested Action**: "Check repository setup or restore template file"
- **Fallback**: Use inline basic structure with warning

**Steering Context Missing**:
- **Warning**: "Steering directory empty or missing - design may not align with project standards"
- **Proceed**: Continue with generation but note limitation in output

**Invalid Requirement IDs**:
  - **Stop Execution**: If requirements.md is missing numeric IDs or uses non-numeric headings (for example, "Requirement A"), stop and instruct the user to fix requirements.md before continuing.

**Spec Gap Found During Design Review**:
- **Stop Execution**: Do not write a patched-over `design.md`
- **User Message**: "Design review found a real spec gap or ambiguity that must be resolved before design can be finalized."
- **Suggested Action**: Clarify or fix `requirements.md`, then re-run `/gsdd-spec-design {feature}`

### Next Phase: Task Generation

**If Design Approved**:
- **Optional**: Run `/gsdd-validate-design {feature}` for interactive quality review
- Run `/gsdd-spec-tasks {feature}` to generate implementation tasks
- Or `/gsdd-spec-tasks {feature} -y` to auto-approve and proceed directly

**If Modifications Needed**:
- Provide feedback and re-run `/gsdd-spec-design {feature}`
- Existing design used as reference (merge mode)
