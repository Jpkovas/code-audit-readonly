# code-audit-readonly skill repo

Repository ready for installation with `npx skills`.

This skill performs strict read-only repository audits and now explicitly uses sub-agents to accelerate broad file-by-file review while preserving deterministic final reporting in `improvements.md`.

## Structure

- `code-audit-readonly/SKILL.md`
- `code-audit-readonly/agents/openai.yaml`

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
