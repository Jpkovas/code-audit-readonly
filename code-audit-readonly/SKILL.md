---
name: code-audit-readonly
description: Use when the user asks for a full read-only repository audit, security/performance/architecture review, file-by-file analysis, or technical debt map where project code must not be changed. Produces one `improvements.md` with traceable findings, sanitized security evidence, progress tracking, complete backlog, and phased remediation plan.
---

# Code Audit Readonly

Audit the current repository without changing project code. The only project artifact this skill may create or update is `improvements.md`.

## Non-negotiables

- Do not edit source code, configs, tests, generated assets, lockfiles, or project metadata.
- Do not run refactors, formatters, migrations, fixers, destructive commands, or commands that intentionally rewrite project files.
- Run the audit end to end without asking for confirmation unless access, safety, or missing context blocks progress.
- Record every validated finding with exact file and line references; do not cap or summarize away repeated locations.
- Redact secrets and credential material everywhere. Report only path, line range, secret class, and sanitized context.
- Use sub-agents for broad audits when available, with one lead auditor owning file tracking, synthesis, numbering, and `improvements.md`.

## Required References

Read these files as soon as the skill triggers:

- [references/audit-workflow.md](references/audit-workflow.md) for the execution sequence, sub-agent packet contract, progress tracking rules, and completion checks.
- [references/report-format.md](references/report-format.md) for allowed categories, severities, finding format, and `improvements.md` structure.

Read [references/review-scope.md](references/review-scope.md) while building the file list and when checking whether security, reliability, tests, dependencies, and performance coverage are complete.

## Lead Auditor Workflow

1. Map the repository with read-only inspection and build a sorted canonical file list covering application code, libraries, tests, configs, scripts, CI, Docker/IaC, migrations, dependency manifests, and documentation that affects runtime or release behavior.
2. Initialize or replace `improvements.md` with system summary, conventions, and one "Progress Tracking" section containing exactly one row per canonical file.
3. Split review packets by ownership boundaries and risk. Keep auth, authorization, routing, shared state, deployment, dependency manifests, and environment/config files visible to a security-focused pass.
4. Review files completely. Prefer parallel sub-agents where available, but keep deterministic synthesis in the lead auditor.
5. Use read-only checks when useful: tests, typecheck, lint, static analysis, dependency/CVE audit, or grep-style evidence collection. If a check writes caches or outputs, redirect outside the audited project or skip it and record the limitation.
6. Merge findings, deduplicate only true duplicates, preserve repeated affected locations, assign final IDs `A001`, `A002`, and so on, and verify no secret value appears in the report.
7. Finish only after every relevant file is marked reviewed, every reviewed file has one `File fully reviewed: <path>` line, all findings are in the backlog and phase plan, and the audited project has no changed artifact except `improvements.md`.

## Sensitive Evidence

For suspected secrets, private keys, tokens, passwords, cookies, connection strings, sensitive endpoints, or credential-bearing logs:

- Confirm the issue by inspection, but never quote the literal value or full offending line.
- Use neutral language such as `hardcoded API credential in config bootstrap`.
- Use placeholders like `<redacted-api-key>` only when a placeholder is needed to explain the risk.
- Preserve traceability through file path and line range, not raw content.

## Final Response

Report the created or updated `improvements.md`, summarize finding counts by severity/category, list any checks that could not run read-only, and confirm that no audited project files were changed other than `improvements.md`.
