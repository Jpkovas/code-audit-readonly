---
name: code-audit-readonly
description: Execute a complete, deterministic, read-only repository audit and produce a single `melhorias.md` action plan with traceable findings (file + lines), severity, category, impact, and high-level fixes. Use when users ask for full code audits, security/performance/architecture reviews, file-by-file analysis, or technical debt mapping without modifying project files.
---

# Code Audit Readonly

Executar auditoria técnica completa de repositório em modo somente leitura e registrar tudo em `melhorias.md`.

## Regras obrigatórias

1. Operar em modo somente leitura para o projeto auditado.
2. Não editar código-fonte, configs ou testes do projeto auditado.
3. Não executar refactors automáticos, formatadores que escrevem em disco, ou comandos destrutivos.
4. Permitir somente a criação/atualização de `melhorias.md` como saída final da auditoria.
5. Não pedir confirmação para prosseguir na auditoria; executar o plano de ponta a ponta.

## Escopo de análise obrigatório

1. Correção e lógica
   1. Detectar bugs óbvios e sutis.
   2. Verificar corridas, estados inconsistentes e uso incorreto de async/concurrency.
   3. Avaliar tratamento de erros/exceções, nullability, tipagem e conversões perigosas.
   4. Cobrir fluxos de borda e cenários extremos.
2. Performance
   1. Encontrar alocações desnecessárias, loops custosos, N+1, I/O excessivo e trabalho repetido.
   2. Verificar oportunidades de cache e estruturas de dados inadequadas.
   3. Correlacionar gargalos entre módulos.
3. Duplicação e manutenibilidade
   1. Detectar duplicação literal e duplicação lógica.
   2. Identificar funções longas, responsabilidades misturadas e acoplamento excessivo.
   3. Apontar APIs internas confusas, nomes ambíguos e comentários desatualizados.
4. Segurança (obrigatório, minucioso)
   1. Segredos hardcoded (tokens, chaves, senhas, endpoints sensíveis).
   2. Vetores de injeção (SQL/NoSQL/command/template).
   3. XSS, CSRF, SSRF, open redirect, path traversal.
   4. Upload inseguro (tipo/tamanho/validação insuficiente).
   5. Autenticação/autorização (bypass, falta de checagem, escalonamento).
   6. Validação/sanitização insuficientes.
   7. Criptografia fraca e hashing inadequado.
   8. Configurações inseguras (CORS amplo, headers ausentes, debug em produção).
   9. Dependências vulneráveis e versões problemáticas.
   10. Vazamento de dados sensíveis em logs, erros e telemetria.
5. Observabilidade e confiabilidade
   1. Validar qualidade dos logs sem expor segredos.
   2. Verificar métricas/tracing quando aplicável.
   3. Avaliar consistência e acionabilidade das mensagens de erro.
6. Testes e qualidade
   1. Identificar lacunas de cobertura em áreas críticas.
   2. Detectar testes frágeis e ausência de integração.
   3. Mapear casos de borda sem teste.

## Método de execução

1. Mapear a árvore do repositório e listar todos os arquivos relevantes:
   1. Código de aplicação, bibliotecas internas, configs, scripts, CI, Docker, IaC, migrations e testes.
2. Inicializar `melhorias.md` com:
   1. Resumo curto do sistema inferido pela estrutura.
   2. Seção "Controle de Progresso" com todos os arquivos relevantes marcados como `pendente`.
   3. Convenção de severidade e categorias.
3. Revisar cada arquivo de forma sequencial e determinística:
   1. Ler o arquivo inteiro.
   2. Registrar achados específicos e correlacionar com imports/chamadas/contratos relacionados.
   3. Atualizar o controle de progresso.
   4. Registrar explicitamente: `Arquivo revisado por completo: <caminho/do/arquivo>`.
4. Executar checagens auxiliares somente leitura quando útil:
   1. Static analysis, linter e typecheck em modo read-only.
   2. Testes sem escrita em disco.
   3. Auditoria de dependências/CVEs.
5. Encerrar `melhorias.md` com:
   1. Top 10 riscos (priorizar segurança e corretude).
   2. Top 10 ganhos de performance (maior custo-benefício).
   3. Plano sugerido por fases:
      1. Fase 1: crítica.
      2. Fase 2: alta.
      3. Fase 3: refino.

## Categorias e severidade

Usar somente estas categorias:
- `Bug`
- `Performance`
- `Segurança`
- `Duplicação`
- `Qualidade de Código`
- `Arquitetura`
- `Manutenibilidade`
- `Observabilidade`
- `Testes`
- `Dependências`

Usar somente estas severidades:
- `Crítica`
- `Alta`
- `Média`
- `Baixa`

## Formato obrigatório de cada achado

Usar IDs únicos sequenciais (`A001`, `A002`, ...).

```text
A0XX
Categoria: <...>
Severidade: <Crítica|Alta|Média|Baixa>
Local: <arquivo>:<linha inicial>-<linha final>
Problema: <descrição objetiva>
Impacto: <impacto real ou potencial>
Sugestão: <correção em alto nível, sem editar código>
Notas de correlação: <arquivos/fluxos relacionados>
Segurança (se aplicável): <cenário de abuso plausível + mitigação>
```

## Requisitos de rastreabilidade

1. Tornar todo achado rastreável para arquivo e linha/trecho.
2. Evitar recomendações genéricas sem evidência.
3. Preferir precisão sobre volume; não omitir achados críticos.
4. Registrar incertezas explicitamente quando a evidência for parcial.

## Critério de conclusão

Concluir apenas quando:
1. Todos os arquivos relevantes estiverem marcados como revisados no Controle de Progresso.
2. Cada arquivo revisado tiver a linha `Arquivo revisado por completo: ...`.
3. `melhorias.md` contiver achados priorizados, top 10 riscos, top 10 ganhos e plano por fases.
4. O projeto auditado permanecer intacto, com apenas `melhorias.md` como artefato de saída da auditoria.
