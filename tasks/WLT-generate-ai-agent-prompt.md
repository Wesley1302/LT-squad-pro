---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist, WLT-ai-agent-product-checklist]
templates: [WLT-ai-agent-prompt-template]
external_skills: [assistant-creator-v2]
---

task:
  name: generateAiAgentPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Page

inputs:
  - { campo: deliverables.ai_agent, tipo: artifact, origem: state, obrigatorio: true, validacao: "WLT-ai-agent-product-checklist=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt pronto para assistant-creator-v2/Claude/Codex." }
  interactive: { enabled: true, prompts: "Confirma a tarefa específica." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "blueprint de agente aprovado", errorMessage: "Não gera agente se o blueprint falhou." }

steps:
  - id: s1
    description: "Preencher WLT-ai-agent-prompt-template: nome, objetivo, usuário ideal, problema, transformação, inputs, perguntas iniciais, fluxo, regras de decisão, formato de saída, validações, limites, anti-alucinação, tom, exemplos, testes simulados, system prompt completo, developer prompt opcional, critérios de aceite."
    validation: "agente resolve TAREFA específica (não brinquedo)"
  - id: s2
    description: "Rodar WLT-ai-agent-product-checklist + WLT-ai-production-prompt-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "prompt de agente gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Agente funciona como PRODUTO low ticket, não como demo genérica."
  - "System prompt completo + anti-alucinação + testes simulados."

errorHandling:
  - { error: "agente genérico", action: "recusar e devolver ao WLT-deliverable-builder" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: ["skill externa: assistant-creator-v2 (construir o agente)"]
