---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist]
templates: [WLT-codex-prompt-template]
---

task:
  name: generateCodexPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: approved_artifact, tipo: artifact, origem: state, obrigatorio: true, validacao: "validation_status=pass", default: null }
  - { campo: target_platform, tipo: enum, origem: usuario, obrigatorio: false, validacao: "-", default: codex }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt para Codex (scripts/automação/planilha com lógica)." }
  interactive: { enabled: true, prompts: "Confirma objetivo técnico." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "artefato de origem aprovado no checklist", errorMessage: "Não gera prompt de produção se o artefato falhou." }

steps:
  - id: s1
    description: "Ler o artefato aprovado. Preencher WLT-codex-prompt-template (objetivo, contexto, inputs, requisitos, output, restrições, critérios de aceite)."
    validation: "template completo"
  - id: s2
    description: "Rodar WLT-ai-production-prompt-quality-checklist."
    validation: "PASS (usa artefato aprovado, não inventa estratégia)"

postConditions:
  - { condition: "prompt gravado em ai_production.prompts_generated", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Prompt executável, sem contradizer oferta/produto/avatar. Não inventa estratégia."

errorHandling:
  - { error: "artefato reprovado", action: "recusar e devolver ao agent dono" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: []
