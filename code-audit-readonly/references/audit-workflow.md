# Audit Workflow Reference

## Coordination Plan

Before reviewing individual files:

1. Build one canonical sorted file list.
2. Normalize paths by removing leading `./`, preserving disk casing, and avoiding trailing slashes.
3. Infer the runtime model: deployed entrypoints, user roles, trusted boundaries, environment modes, generated code, test-only code, and local-only tooling.
4. Split files into packets by domain, risk, and dependency boundaries.
5. Assign every file to the lead auditor or a named sub-agent packet.
6. Keep a security/dependency/config packet covering manifests, lockfiles, environment templates, CI, deployment, auth, authorization, routing, and external inputs.

## Sub-Agent Packet Contract

Use this contract whenever more than one meaningful file or subsystem is in scope and sub-agents are available:

```text
Stay read-only. Review only these files/subsystem boundaries completely.
For each file, return:
- reviewed_files
- candidate_findings with exact file:line-line evidence
- reachability for each candidate: actor, normal entrypoint, prerequisites, and deployed/runtime mode
- cross_file_notes
- uncertainties
- rejected_candidates that looked concerning but failed the real-world finding gate
- suggested_followups
- File fully reviewed: <path/to/file>
Do not edit improvements.md. Redact all secrets and credential material.
```

The lead auditor reconciles every packet into the final report or records that the packet produced no validated findings.

## Candidate Validation

Before promoting any candidate to a finding:

1. Trace how normal production, CI, release, admin, or documented user behavior reaches the code.
2. Identify the actor and privilege level required before the problem occurs.
3. Check surrounding guards, schema validation, feature flags, build steps, deployment config, and caller contracts.
4. Confirm the impact is observable: incorrect result, security boundary failure, data loss/exposure, operational failure, measurable performance issue, or maintenance risk on an active path.
5. Reject candidates that depend on impossible input, dead code, test fixtures, examples, disabled features, local-only scripts with no release effect, or an attacker who already has equivalent control.

If the evidence is plausible but incomplete, record it as an uncertainty or follow-up instead of a numbered finding.

## Progress Tracking Rules

- Keep exactly one "Progress Tracking" section.
- Keep exactly one progress row per canonical file path.
- Update rows in place from `pending` to `in_progress` to `reviewed`.
- Write `File fully reviewed: <path/to/file>` exactly once per file.
- If a file is revisited, add notes under the same file entry instead of creating another progress row.

Before finishing, validate:

- Number of reviewed rows equals number of unique relevant files.
- Every finding location appears in a reviewed file.
- Every sub-agent packet was reconciled.
- Cross-file notes and disagreements were resolved or recorded as uncertainty.

## Read-Only Checks

Useful checks include tests, typecheck, linter, static analysis, dependency audit, manifest validation, and focused search. Run them only when they do not mutate the audited project. If a tool writes caches or artifacts, either redirect those outputs outside the project or record why the check was skipped.

## Completion Gate

Finish only when:

1. All relevant files are reviewed in progress tracking.
2. Each reviewed file has one `File fully reviewed: ...` line.
3. `improvements.md` contains complete findings, prioritized backlog, detailed phase plan, and completeness checkpoint.
4. The audited project remains intact, with only `improvements.md` as the audit artifact.
