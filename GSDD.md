# Agentic SDLC and Spec-Driven Development

gsdd: Spec-Driven Development on an agentic SDLC

## Project Context

### Paths
- Steering: `docs/steering/`
- Specs: `docs/specs/`

### Steering vs Specification

**Steering** (`docs/steering/`) - Guide AI with project-wide rules and context
**Specs** (`docs/specs/`) - Formalize development process for individual features

### Active Specifications
- Check `docs/specs/` for active specifications
- Use `/gsdd-spec-status [feature-name]` to check progress

## Minimal Workflow
- Getting started depends on the project:
  - New project: `/gsdd-discovery "idea"` first — the first requirements analysis; confirmed product, technology, and structure decisions are recorded into steering through `gsdd-steering`. Once code exists, `/gsdd-steering` keeps steering in sync with it.
  - Existing project: `/gsdd-steering` first (bootstrap steering from the codebase), then `/gsdd-discovery "idea"`
- Optional: `/gsdd-steering custom <topic>` for specialized steering documents
- Discovery: `/gsdd-discovery "idea"` — determines action path, writes brief.md + spec.json for each new spec and roadmap.md (development plan) for multi-spec projects, and records confirmed project-level decisions into steering
- Phase 1 (Specification):
  - Single spec: `/gsdd-spec-quick {feature} [--auto]` or step by step:
    - `/gsdd-spec-requirements {feature}`
    - `/gsdd-spec-design {feature} [-y]`
    - `/gsdd-validate-design {feature}` (optional: design review)
    - `/gsdd-spec-tasks {feature} [-y]`
  - Multi-spec: `/gsdd-spec-batch` — creates all specs from roadmap.md in parallel by dependency wave
- Phase 2 (Implementation): `/gsdd-impl {feature} [tasks] [--review required|inline|off]`
  - Without task numbers: autonomous mode (subagent per task + independent review + final validation)
  - With task numbers: manual mode (selected tasks in main context, still reviewer-gated before completion)
  - `--review off` skips task-local review; use it intentionally and keep `/gsdd-validate-impl {feature}` as the final quality gate
  - `/gsdd-validate-impl {feature}` (standalone re-validation)
- Progress check: `/gsdd-spec-status {feature}` (use anytime)

## Skills Structure
Skills are located in `.claude/skills/gsdd-*/SKILL.md`
- Each skill is a directory with a `SKILL.md` file
- Skills run inline with access to conversation context
- Skills may delegate parallel research to subagents for efficiency
- Additional files (templates, examples) can be added to skill directories
- `gsdd-review` — task-local adversarial review protocol used by reviewer subagents
- `gsdd-debug` — root-cause-first debug protocol used by debugger subagents
- `gsdd-verify-completion` — fresh-evidence gate before success or completion claims
- Use skills explicitly requested by the user and skills relevant to the task's domain, including design, accessibility, and UX.
- Select skills from their descriptions or metadata first, then read only the selected skills and the references needed for the task.
- Follow explicit host and project rules and retain required workflow checks. Do not skip relevant skills just because the task is small.

## Development Rules
- 3-phase approval workflow: Requirements → Design → Tasks → Implementation
- Human review required each phase; use `-y` only for intentional fast-track
- Keep steering current and verify alignment with `/gsdd-spec-status`
- Follow the user's instructions precisely, and within that scope act autonomously: gather the necessary context and complete the requested work end-to-end in this run, asking questions only when essential information is missing or the instructions are critically ambiguous.

## Steering Configuration
- For spec and implementation work, load the core steering files below from `docs/steering/`. Reuse current context rather than rereading unchanged files.
- Load additional steering only when required by project rules or relevant to the task.
- Default files: `product.md`, `tech.md`, `structure.md`
- Custom files are supported (managed via `/gsdd-steering custom <topic>`)
