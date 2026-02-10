# code-audit-readonly skill repo

Repositório pronto para instalação com `npx skills`.

## Estrutura

- `code-audit-readonly/SKILL.md`
- `code-audit-readonly/agents/openai.yaml`

## Instalação via npx skills

Depois de publicar este repositório no GitHub:

```bash
npx skills add <owner>/<repo>
```

Para instalar direto sem prompt interativo:

```bash
npx skills add <owner>/<repo> --skill code-audit-readonly -y
```

## Verificação rápida

Listar skills disponíveis no repositório:

```bash
npx skills add <owner>/<repo> --list
```
