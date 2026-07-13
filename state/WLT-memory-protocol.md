# Memory Protocol — LT-squad-pro

Como os agents leem e escrevem estado. O state é a única fonte de verdade entre etapas.

## Ordem de leitura (todo agent, no início da task)
1. Ler `state/WLT-lt-journey-state.yaml` (jornada completa).
2. Ler `state/WLT-session-state.json` (sessão atual: agent/task ativos, menu pendente).
3. Confirmar que os `required_artifacts` do handoff existem. Se não existem → **não executar**, devolver para `WLT-lt-chief` com o pré-requisito ausente.

## Ordem de escrita (todo agent, no fim da task)
1. Gravar o artefato no caminho do state indicado nos `outputs` da task.
2. Registrar a task em `journey.completed_steps` (ou `failed_steps`).
3. Atualizar `journey.current_agent` / `current_task` para o próximo handoff.
4. Atualizar `WLT-session-state.json` (`last_artifact_produced`, `last_checklist_result`, `pending_menu`).

## Evidence Ledger
- Toda descoberta de público entra em `avatar.evidence_ledger` no formato de `templates/WLT-evidence-ledger-template.yaml`.
- Sem `source_url` verificável → `confidence: baixa` e marcado como **hipótese**. Nunca vira "fato".

## Bypass
Se o usuário escrever exatamente **"quero pular com risco documentado"**, gravar em `journey.bypasses`:
```yaml
- etapa_pulada: <task ou etapa>
  risco: <o que pode dar errado>
  data: <YYYY-MM-DD>
  autorizado_pelo_usuario: true
```
Bypass NÃO limpa o pré-requisito — só registra que o usuário assumiu o risco.

## Resultados de checklist
Todo checklist grava um dos três: `PASS` (avança) · `FAIL` (volta para task anterior) · `BYPASS` (avança com risco documentado).
O resultado vai para `<secao>.validation_status` e para `WLT-session-state.json.last_checklist_result`.

## Regra de não-alucinação
- Nunca inventar prova, pesquisa, número ou depoimento.
- Diferenciar sempre: `existe-no-metodo` | `convencao-observada` | `extensao-lt-squad-pro`.
- Se faltar input essencial: PARAR e pedir o dado específico, não preencher com genérico.
