# Steering Principles

Steering files are **project memory**, not exhaustive specifications.

---

## Content Granularity

### Golden Rule
> "If new code follows existing patterns, steering shouldn't need updating."

### ✅ Document
- Organizational patterns (feature-first, layered)
- Naming conventions (PascalCase rules)
- Import strategies (absolute vs relative)
- Architectural decisions (state management)
- Technology standards (key frameworks)

### ❌ Avoid
- Complete file listings
- Every component description
- All dependencies
- Implementation details
- Agent-specific tooling directories (e.g. `.cursor/`, `.gemini/`, `.claude/`)
- Detailed documentation of the SDD workflow files (skills, rules, and templates under `.claude/skills/`)

### Example Comparison

**Bad** (Specification-like):
```markdown
- /contracts/session-open - Session open request and response
- /contracts/session-close - Session close notification
- /contracts/file-chunk - File transfer chunk
... (50+ entries)
```

**Good** (Project Memory):
```markdown
## Shared Contracts (`/contracts/`)
Canonical interface and message definitions shared across components
- One file per contract area, named by function
- Versioned; breaking changes require a new version
- No business logic
```

---

## Security

Never include:
- API keys, passwords, credentials
- Database URLs, internal IPs
- Secrets or sensitive data

---

## Quality Standards

- **Single domain**: One topic per file
- **Concrete examples**: Show patterns with code
- **Explain rationale**: Why decisions were made
- **Maintainable size**: 100-200 lines typical

---

## Preservation (when updating)

- Preserve user sections and custom examples
- Additive by default (add, don't replace)
- Do not add timestamps or change logs to steering files; version history lives in git
- Explain why changes were made in the summary to the user

---

## Notes

- Templates define the sections of each core steering file; fill them with project facts and do not add sections they do not define
- Points known to be undecided go in an "Open Questions" section at the end of the file (core or custom), not into other sections as conclusions; it is the only section allowed beyond the templates
- Follow same granularity principles as core steering
- All steering files loaded as project memory
- Light references to `docs/specs/` and `docs/steering/` are acceptable; avoid documenting the SDD workflow files under `.claude/skills/`
- Custom files equally important as core files

---

## File-Specific Focus

- **product.md**: Purpose, value, business context (not exhaustive features)
- **tech.md**: Key frameworks, standards, conventions (not all dependencies)
- **structure.md**: Organization patterns, naming rules (not directory trees; do not enumerate every module or package, describe the pattern and give a representative example)
- **Custom files**: Specialized patterns (API, testing, security, etc.)
