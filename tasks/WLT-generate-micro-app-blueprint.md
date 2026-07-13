---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-deliverable-quality-checklist]
templates: [WLT-deliverable-blueprint-template]
---

task:
  name: generateMicroAppBlueprint()
  responsavel: WLT-deliverable-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: deliverable_blueprint, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: deliverables.micro_app, tipo: artifact, destino: state.deliverables.micro_app, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Define problema, usuário, inputs, outputs, fluxo, telas, lógica, edge cases." }
  interactive: { enabled: true, prompts: "Confirma inputs/outputs (bloqueante se não claros)." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "caso de uso + inputs/outputs claros", errorMessage: "Micro app não é gerado sem caso de uso e inputs/outputs claros." }

steps:
  - id: s1
    description: "Definir problema, usuário ideal, inputs, outputs, fluxo, telas, lógica, validações, edge cases, stack sugerida."
    validation: "resolve 1 tarefa em <2min"
  - id: s2
    description: "Rodar WLT-deliverable-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "micro_app blueprint gravado", errorMessage: "Incompleto." }

acceptanceCriteria:
  - "Uma tarefa, mobile-first, edge cases tratados."

errorHandling:
  - { error: "inputs/outputs vagos", action: "bloquear e pedir definição" }

handoff:
  next_agent: WLT-ai-production-prompt-engineer
  next_task: WLT-generate-micro-app-prompt
  condition: "WLT-deliverable-quality-checklist PASS e usuário escolhe produzir"
  alternatives: ["continuar -> WLT-write-sales-page"]
