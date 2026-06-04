# Report Format Reference

## Structure

Use this structure for `improvements.md`:

```markdown
# improvements.md

## 1. System summary
- Inferred architecture and main modules.
- Main risk surfaces.

## 2. Conventions
- Allowed categories and severity scale.
- Finding ID convention: A001, A002, ...

## 3. Progress Tracking
- [ ] path/to/file-a.ext
- [ ] path/to/file-b.ext

## 4. Complete finding inventory
### A001
Category: ...
Severity: ...
Location: ...
Problem: ...
Impact: ...
Suggestion: ...
Correlation notes: ...
Security (if applicable): ...

## 5. Prioritized backlog (all findings)

## 6. Detailed phased remediation plan

## 7. Completeness checkpoint
```

## Categories

Use only:

- `Bug`
- `Performance`
- `Security`
- `Duplication`
- `Code Quality`
- `Architecture`
- `Maintainability`
- `Observability`
- `Tests`
- `Dependencies`

## Severities

Use only:

- `Critical`
- `High`
- `Medium`
- `Low`

## Finding Format

```text
A0XX
Category: <allowed category>
Severity: <Critical|High|Medium|Low>
Location: <file>:<start line>-<end line>
Problem: <objective description>
Impact: <real or potential impact>
Suggestion: <high-level fix, without editing code>
Correlation notes: <related files/flows>
Security (if applicable): <plausible abuse scenario and mitigation>
```

Secret-related findings use the same structure, but never include raw secret values or full source lines containing sensitive material.

## Detailed Remediation Plan

Include:

1. Planning assumptions and constraints, including read-only audit boundaries and unknowns.
2. Complete prioritized backlog with every finding ID, effort estimate `S`, `M`, or `L`, rationale, and primary risk type.
3. Phase plan with objective, included finding IDs, dependencies, validation gates, and exit criteria.
4. Sequencing rules: critical and exploitable high security/correctness first; performance and maintainability after risk containment unless blocking.
5. Delivery roadmap by batch/wave, expected risk reduction, and verification focus.
