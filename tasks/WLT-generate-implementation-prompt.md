---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist]
templates: [WLT-implementation-prompt-template]
---

task:
  name: generateImplementationPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: approved_artifact, tipo: artifact, origem: state, obrigatorio: true, validacao: "validation_status=pass", default: null }
  - { campo: target_platform, tipo: enum, origem: usuario, obrigatorio: false, validacao: "-", default: outra_llm }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Prompt genérico de implementação para qualquer artefato aprovado (pack, template, script, protocolo, planilha, dashboard, automação)." }
  interactive: { enabled: true, prompts: "Confirma plataforma e formato de saída." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "artefato aprovado", errorMessage: "Não gera prompt se o artefato falhou no checklist." }

steps:
  - id: s1
    description: "Preencher WLT-implementation-prompt-template: objetivo, contexto (do state), inputs, requisitos, formato de saída, restrições técnicas/design/copy, checklist de validação, prompt final."
    validation: "template completo, coerente com o artefato"
  - id: s2
    description: "Rodar WLT-ai-production-prompt-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "prompt gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Prompt executável na plataforma escolhida. Não inventa estratégia. Coerente com oferta/produto/avatar."

errorHandling:
  - { error: "artefato reprovado", action: "recusar" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: []
