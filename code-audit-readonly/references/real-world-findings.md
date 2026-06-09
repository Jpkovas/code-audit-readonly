# Real-World Findings Reference

Use this gate before promoting any candidate into `improvements.md`.

## Reportable Finding Gate

A numbered finding must satisfy all of these:

1. **Reachable path**: normal user behavior, documented admin/operator behavior, deployed service traffic, supported integration, CI/release, or migration can execute the path.
2. **Plausible actor**: the actor has only the privileges expected for that scenario. Do not assume source edits, shell access, database writes, or config control unless that is the audited workflow.
3. **Real input or state**: the triggering input, data volume, timing, or state can be produced by normal use or a credible external interaction.
4. **Observable impact**: the result is a user-visible bug, data loss/exposure, authorization failure, operational outage, release risk, measurable performance problem, or maintenance risk on active code.
5. **Code evidence**: file and line references show both the risky behavior and enough surrounding context to justify why existing guards do not prevent it.

If any point is missing, do not create a numbered finding. Record it as an uncertainty only if it is useful for the user to investigate later.

## Common Non-Findings

Do not report these as findings by themselves:

- Private helper misuse when all current callers pass validated values.
- Test fixture, storybook, seed, demo, generated, or example code that cannot ship or affect release behavior.
- Dev-only configuration that is clearly excluded from production and cannot leak credentials or weaken deployed behavior.
- A missing guard that would matter only after an attacker already has equivalent control, such as arbitrary code execution, write access to trusted config, or database administrator privileges.
- Hypothetical scale problems without a plausible production data size, call frequency, or user workflow.
- Missing tests for code that is trivial, inactive, generated, or already protected by stronger integration coverage.
- Style or abstraction preferences without concrete defect risk, ownership confusion, repeated bug history, or active maintenance cost.

## Severity Calibration

- `Critical`: reachable in production or release flow and can cause broad compromise, irreversible data loss, severe outage, or systemic tenant/user boundary failure.
- `High`: reachable and likely to cause significant security, correctness, data integrity, or release failure for a meaningful user/operator path.
- `Medium`: reachable and credible, but limited by prerequisites, blast radius, recoverability, frequency, or affected audience.
- `Low`: real issue on active code with small impact, maintainability drag, observability gap, or test risk tied to a concrete workflow.

Lower severity when exploitability depends on rare timing, privileged operators, unusual but supported configuration, or manually created data. Do not raise severity based only on generic vulnerability class names.

## Evidence Standard

Each finding should answer:

- Who triggers it?
- Through which normal entrypoint?
- What assumptions must hold?
- Why do existing guards not stop it?
- What user, operator, security, or release impact follows?
- How could a maintainer verify the issue without changing code?

When these answers are not available, the candidate is not ready for the final inventory.
