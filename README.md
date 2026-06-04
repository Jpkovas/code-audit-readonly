# code-audit-readonly skill repo

Repository ready for installation with `npx skills`.

This skill performs strict read-only repository audits and uses sub-agents to accelerate broad file-by-file review while preserving deterministic final reporting in `improvements.md`.

The active `SKILL.md` is intentionally compact. Detailed audit workflow, report format, and review-scope guidance live in reference files so Codex loads the heavy material only when the skill is invoked.

## Structure

- `code-audit-readonly/SKILL.md`
- `code-audit-readonly/agents/openai.yaml`
- `code-audit-readonly/references/audit-workflow.md`
- `code-audit-readonly/references/report-format.md`
- `code-audit-readonly/references/review-scope.md`

## Installation via npx skills

After publishing this repository to GitHub:

```bash
npx skills add Jpkovas/code-audit-readonly
```

To install directly without an interactive prompt:

```bash
npx skills add Jpkovas/code-audit-readonly --skill code-audit-readonly -y
```

## Quick verification

List skills available in the repository:

```bash
npx skills add Jpkovas/code-audit-readonly --list
```
