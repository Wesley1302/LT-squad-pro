# Guia de Uso — LT-squad-pro

## 1. Instalar
Copie a pasta `LT-squad-pro/` para o seu runtime de squads (padrão AIOX-core / Claude Code).
O ponto de entrada é sempre `WLT-lt-chief` (`config.yaml → activation.entry`).

## 2. Ativar
Carregue `config.yaml`. O `WLT-lt-chief` inicia com `WLT-run-initial-diagnosis` e o greeting:
> "Me diga em uma frase onde você está: (a) não sei o que vender, (b) tenho nicho, (c) tenho produto, (d) tenho tráfego sem venda, (e) tenho venda e quero escalar."

## 2b. Comandos visíveis (camada lt-*)
Todos os comandos do usuário têm prefixo `lt-` (`.claude/commands/lt-*.md`; mapa em `config.yaml -> commands`).
**`/lt-chief` é a entrada única** — diagnostica o estágio e conduz; o usuário não precisa saber qual agente chamar.
Etapas diretas: `/lt-produto`, `/lt-order-bumps`, `/lt-oferta`, `/lt-pagina`, `/lt-criativos`, `/lt-rastreamento`, `/lt-trafego`, `/lt-gargalo`, `/lt-escala`, `/lt-prompt-landing` etc.
Para uso global no Claude Code: copie `.claude/commands/lt-*.md` para `~/.claude/commands/`.

## 3. Rodar um workflow
O diagnóstico escolhe o workflow. Manualmente, chame o workflow por `id` (ver `docs/WLT-workflow-map.md`).
Cada task lê o state, executa `steps`, grava o artefato e faz o handoff se o checklist der PASS.

## 4. Como funcionam os Agents
Markdown com frontmatter (`agents/*.md`): id, whenToUse, persona, commands, dependencies, tools, restrictions,
handoff behavior, memory behavior, output expectations. O agent NÃO executa fora das suas restrictions.

## 5. Como funcionam as Tasks
YAML (`tasks/*.md`): inputs → preConditions → steps → postConditions → acceptanceCriteria → handoff.
A Task é a unidade executável reutilizável.

## 6. Como funciona o State
`state/WLT-lt-journey-state.yaml` é a fonte de verdade. `state/WLT-memory-protocol.md` define ordem de leitura/escrita,
Evidence Ledger, bypass e resultados de checklist. Inicialize novas instâncias a partir de `templates/WLT-journey-state-template.yaml`.

## 7. Prompts de produção
Após qualquer artefato aprovado, escolha 2/3/4/5 no menu. O `WLT-ai-production-prompt-engineer` gera o prompt
pela task/template certos (ver `docs/WLT-ai-production-layer.md`). Bloqueia se o artefato falhou no checklist.

## 8. SKILL.md (versionado)
O `SKILL.md` na raiz do repo é a projeção do agent de entrada `WLT-lt-chief` — versionado para: (a) instalar = copiar o repo; (b) fonte única de verdade; (c) auditável no git. Regenere ao mudar comandos/ordem/gates.

## 8b. Gerar outras projeções SKILL.md (opcional)
O `SKILL.md` é uma **projeção** de um Agent. Para gerar:
1. Escolha o Agent (ex: `agents/WLT-product-miojo-builder.md`).
2. Crie `SKILL.md` com frontmatter `name`/`description` a partir de `id` + `whenToUse` do agent.
3. No corpo, referencie as Tasks do agent (dependencies.tasks) como o passo a passo executável.
4. Aponte os checklists/templates/data como recursos.
Regra: a lógica canônica vive na Task; o `SKILL.md` só projeta o ponto de entrada. Ao mudar a Task, regenere o SKILL.

## 9. Exemplos de uso
Ver `tests/case-01..08`. Ex.: "Não sei o que vender" → WLT-lt-from-zero; "Gastei R$300 e não vendi" → WLT-funnel-diagnosis-cycle.

## 10. Troubleshooting
| Sintoma | Causa | Ação |
|---|---|---|
| Agent recusa avançar | required_artifact ausente ou checklist FAIL | complete a etapa anterior |
| "Não gero landing" | oferta/página reprovadas | volte a WLT-offer-build-cycle / WLT-write-sales-page |
| Persona genérica | sem evidência real | rode WLT-research-avatar / WLT-extract-voice-of-customer |
| Quer escalar e trava | ROAS<2 | WLT-funnel-diagnosis-cycle (corrigir rota) |
| Sem web/tools | runtime sem ferramentas | agents caem no modo manual (tool-overrides.yaml) |
| Quero pular etapa | bloqueio | escreva "quero pular com risco documentado" (grava bypass) |
