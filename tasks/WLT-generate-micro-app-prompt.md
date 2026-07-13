---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist]
templates: [WLT-micro-app-prompt-template]
---

task:
  name: generateMicroAppPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Page

inputs:
  - { campo: deliverables.micro_app, tipo: artifact, origem: state, obrigatorio: true, validacao: "WLT-deliverable-quality-checklist=pass", default: null }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt de micro app para Lovable/Bolt/Replit/Cursor." }
  interactive: { enabled: true, prompts: "Confirma inputs/outputs e telas." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "caso de uso + inputs/outputs claros", errorMessage: "Não gera micro app sem caso de uso e inputs/outputs claros." }

steps:
  - id: s1
    description: "Preencher WLT-micro-app-prompt-template: problema, usuário ideal, inputs, outputs, fluxo, telas, lógica, validações, stack, UI/UX, responsividade, edge cases, exemplos de dados, critérios de pronto."
    validation: "resolve 1 tarefa"
  - id: s2
    description: "Rodar WLT-ai-production-prompt-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "prompt de micro app gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Micro app de tarefa única, mobile-first, edge cases tratados."

errorHandling:
  - { error: "inputs/outputs vagos", action: "recusar e devolver ao WLT-deliverable-builder" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: []
