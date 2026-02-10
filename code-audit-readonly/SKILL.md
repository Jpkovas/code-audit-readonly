---
name: code-audit-readonly
description: Execute a complete, deterministic, read-only repository audit and produce a single `improvements.md` action plan with traceable findings (file + lines), severity, category, impact, and high-level fixes. Use when users ask for full code audits, security/performance/architecture reviews, file-by-file analysis, or technical debt mapping without modifying project files.
---

# Code Audit Readonly

Run a full technical repository audit in read-only mode and record everything in `improvements.md`.

## Mandatory rules

1. Operate in read-only mode for the audited project.
2. Do not edit source code, configs, or tests in the audited project.
3. Do not run automatic refactors, formatters that write to disk, or destructive commands.
4. Allow only the creation/update of `improvements.md` as the final audit output.
5. Do not ask for confirmation to proceed with the audit; execute the plan end to end.

## Mandatory analysis scope

1. Correctness and logic
   1. Detect obvious and subtle bugs.
   2. Check for race conditions, inconsistent states, and incorrect async/concurrency usage.
   3. Evaluate error/exception handling, nullability, typing, and unsafe conversions.
   4. Cover edge flows and extreme scenarios.
2. Performance
   1. Find unnecessary allocations, expensive loops, N+1 patterns, excessive I/O, and repeated work.
   2. Check for caching opportunities and inappropriate data structures.
   3. Correlate bottlenecks across modules.
3. Duplication and maintainability
   1. Detect literal duplication and logical duplication.
   2. Identify long functions, mixed responsibilities, and excessive coupling.
   3. Flag confusing internal APIs, ambiguous names, and outdated comments.
4. Security (mandatory, thorough)
   1. Hardcoded secrets (tokens, keys, passwords, sensitive endpoints).
   2. Injection vectors (SQL/NoSQL/command/template).
   3. XSS, CSRF, SSRF, open redirect, path traversal.
   4. Insecure uploads (insufficient type/size/validation checks).
   5. Authentication/authorization issues (bypass, missing checks, privilege escalation).
   6. Insufficient validation/sanitization.
   7. Weak cryptography and inadequate hashing.
   8. Insecure configurations (overly broad CORS, missing headers, debug in production).
   9. Vulnerable dependencies and problematic versions.
   10. Sensitive data leakage in logs, errors, and telemetry.
5. Observability and reliability
   1. Validate log quality without exposing secrets.
   2. Verify metrics/tracing where applicable.
   3. Evaluate consistency and actionability of error messages.
6. Tests and quality
   1. Identify coverage gaps in critical areas.
   2. Detect brittle tests and missing integration coverage.
   3. Map untested edge cases.

## Execution method

1. Map the repository tree and list all relevant files:
   1. Application code, internal libraries, configs, scripts, CI, Docker, IaC, migrations, and tests.
2. Initialize `improvements.md` with:
   1. A short system summary inferred from the structure.
   2. A "Progress Tracking" section with all relevant files marked as `pending`.
   3. Severity and category conventions.
3. Review each file sequentially and deterministically:
   1. Read the entire file.
   2. Record specific findings and correlate with related imports/calls/contracts.
   3. Update progress tracking.
   4. Explicitly record: `File fully reviewed: <path/to/file>`.
4. Run read-only auxiliary checks when useful:
   1. Static analysis, linter, and typecheck in read-only mode.
   2. Run tests without writing to disk.
   3. Dependency/CVE audit.
5. Close `improvements.md` with:
   1. Top 10 risks (prioritize security and correctness).
   2. Top 10 performance gains (highest cost-benefit).
   3. Suggested phased plan:
      1. Phase 1: critical.
      2. Phase 2: high.
      3. Phase 3: refinement.

## Categories and severity

Use only these categories:
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

Use only these severity levels:
- `Critical`
- `High`
- `Medium`
- `Low`

## Mandatory format for each finding

Use unique sequential IDs (`A001`, `A002`, ...).

```text
A0XX
Category: <...>
Severity: <Critical|High|Medium|Low>
Location: <file>:<start line>-<end line>
Problem: <objective description>
Impact: <real or potential impact>
Suggestion: <high-level fix, without editing code>
Correlation notes: <related files/flows>
Security (if applicable): <plausible abuse scenario + mitigation>
```

## Traceability requirements

1. Make every finding traceable to file and line/section.
2. Avoid generic recommendations without evidence.
3. Prefer precision over volume; do not omit critical findings.
4. Explicitly record uncertainties when evidence is partial.

## Completion criteria

Finish only when:
1. All relevant files are marked as reviewed in Progress Tracking.
2. Each reviewed file has the line `File fully reviewed: ...`.
3. `improvements.md` contains prioritized findings, top 10 risks, top 10 gains, and a phased plan.
4. The audited project remains intact, with only `improvements.md` as the audit output artifact.
