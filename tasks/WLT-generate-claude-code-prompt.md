---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist]
templates: [WLT-claude-code-prompt-template]
---

task:
  name: generateClaudeCodePrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: approved_artifact, tipo: artifact, origem: state, obrigatorio: true, validacao: "validation_status=pass", default: null }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt para Claude Code (componentes, refactor, app, design system)." }
  interactive: { enabled: true, prompts: "Confirma escopo técnico." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "artefato aprovado", errorMessage: "Não gera prompt se o artefato falhou no checklist." }

steps:
  - id: s1
    description: "Preencher WLT-claude-code-prompt-template com contexto, arquivos alvo, requisitos, restrições, critérios de aceite."
    validation: "template completo"
  - id: s2
    description: "Rodar WLT-ai-production-prompt-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "prompt gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Executável no Claude Code. Não inventa estratégia. Aponta skill frontend-design se for UI."

errorHandling:
  - { error: "artefato reprovado", action: "recusar" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: []
