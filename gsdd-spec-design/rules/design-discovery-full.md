# Full Discovery Process for Technical Design

## Objective
Conduct comprehensive research and analysis to ensure the technical design is based on complete, accurate, and up-to-date information.

## Discovery Steps

### 1. Requirements Analysis
**Map Requirements to Technical Needs**
- Extract all functional requirements from EARS format
- Identify non-functional requirements (performance, security, scalability)
- Determine technical constraints and dependencies
- List core technical challenges

### 2. Existing Implementation Analysis
**Understand Current System** (if modifying/extending):
- Analyze codebase structure and architecture patterns
- Map reusable components, services, utilities
- Identify domain boundaries and data flows
- Document integration points and dependencies
- Map each requirement to the existing code that serves it, tagging what remains as Missing (not in the code), Unknown (needs investigation), or Constraint (existing architecture limits the approach); record the map in `research.md`
- Determine approach: extend vs refactor vs wrap

### 3. Technology Research
**Only for technologies this design introduces, or decisions that depend on facts you are unsure of**:
- Use WebSearch to locate official documentation, specifications, and source repositories
- Use WebFetch to read them: API references, usage examples, migration guides, breaking changes, known issues

### 4. External Dependencies Investigation
**For Each External Service/Library**:
- Read its official documentation and source repository
- Verify API signatures and authentication methods
- Check version compatibility with existing stack
- Check usage constraints, such as limits and licensing
- Document security considerations
- Note any gaps requiring implementation investigation

### 5. Architecture Pattern & Boundary Analysis
**Evaluate Architectural Options**:
- Compare relevant patterns (MVC, Clean, Hexagonal, Event-driven)
- Assess fit with existing architecture and steering principles
- Identify domain boundaries and ownership seams required to avoid team conflicts
- Consider scalability implications and operational concerns
- Evaluate maintainability
- Document preferred pattern and rejected alternatives in `research.md`

### 6. Risk Assessment
**Identify Technical Risks**:
- Performance bottlenecks and scaling limits
- Security vulnerabilities and attack vectors
- Integration complexity and coupling
- Technical debt creation vs resolution

## Research Guidelines
- Research only what the design depends on: external libraries, services, or protocols it introduces, and facts you are unsure of. Do not research what steering or the existing codebase already settles.
- Prefer primary sources: official documentation, specifications, and source repositories.
- Record sources in `research.md` so decisions can be traced.

## Output Requirements
Capture all findings that impact design decisions in `research.md` using the shared template:
- Key insights affecting architecture, technology alignment, and contracts
- Constraints discovered during research
- Recommended approaches and selected architecture pattern with rationale
- Rejected alternatives and trade-offs (documented in the Design Decisions section)
- Updated domain boundaries that inform the components and their contracts
- Risks and mitigation strategies
- Gaps requiring further investigation during implementation