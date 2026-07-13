---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist]
templates: [WLT-claude-code-prompt-template]
external_skills: [frontend-design]
---

task:
  name: generateDesignSystemPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: sales_page, tipo: artifact, origem: state, obrigatorio: true, validacao: "page.validation_status=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt de design system / direção visual." }
  interactive: { enabled: true, prompts: "Confirma a estética alvo." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "página aprovada", errorMessage: "Design system deriva de página/oferta aprovadas." }

steps:
  - id: s1
    description: "Definir tokens (cores, tipografia, espaçamento), componentes, hierarquia, mobile-first, premium (refined gradients, subtle glassmorphism)."
    validation: "coerente com WLT-landing-page-tech-stack.yaml"
  - id: s2
    description: "Preencher WLT-claude-code-prompt-template com foco em design system. Rodar checklist."
    validation: "PASS"

postConditions:
  - { condition: "prompt de design gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Design system consistente, mobile-first, conversion-focused. Aponta skill frontend-design."

errorHandling:
  - { error: "estética que atrapalha conversão", action: "priorizar conversão" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: ["skill externa: frontend-design"]
