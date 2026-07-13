---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-ai-agent-product-checklist, WLT-deliverable-quality-checklist]
templates: [WLT-ai-agent-blueprint-template]
---

task:
  name: generateAiAgentBlueprint()
  responsavel: WLT-deliverable-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: deliverable_blueprint, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: deliverables.ai_agent, tipo: artifact, destino: state.deliverables.ai_agent, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Define tarefa específica, inputs, fluxo, regras, saída, anti-alucinação." }
  interactive: { enabled: true, prompts: "Confirma a TAREFA que o agente resolve." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "produto é agente de IA", errorMessage: "Use só quando o produto for agente/GPT/assistente." }

steps:
  - id: s1
    description: "Definir a TAREFA específica que o agente resolve (não brinquedo genérico)."
    validation: "tarefa concreta e paga"
  - id: s2
    description: "Definir usuário ideal, inputs obrigatórios, perguntas iniciais, fluxo, regras de decisão, formato de saída, limites, anti-alucinação, tom."
    validation: "WLT-ai-agent-product-checklist PASS"

postConditions:
  - { condition: "ai_agent blueprint gravado", errorMessage: "Blueprint de agente incompleto." }

acceptanceCriteria:
  - "Agente funciona como PRODUTO (resolve tarefa), não como demo."

errorHandling:
  - { error: "agente genérico", action: "recortar até uma tarefa específica" }

handoff:
  next_agent: WLT-ai-production-prompt-engineer
  next_task: WLT-generate-ai-agent-prompt
  condition: "WLT-ai-agent-product-checklist PASS e usuário escolhe produzir"
  alternatives: ["continuar -> WLT-write-sales-page", "skill externa: assistant-creator-v2"]
