---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-ebook-quality-checklist, WLT-deliverable-quality-checklist]
templates: [WLT-ebook-blueprint-template]
---

task:
  name: generateEbookBlueprint()
  responsavel: WLT-deliverable-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: deliverable_blueprint, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: deliverables.ebook, tipo: artifact, destino: state.deliverables.ebook, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera sumário + objetivo por capítulo + exercícios." }
  interactive: { enabled: true, prompts: "Confirma o sumário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "deliverable_blueprint existe", errorMessage: "Rode WLT-create-deliverables-blueprint antes." }

steps:
  - id: s1
    description: "Definir sumário objetivo (20-80 páginas), objetivo de cada capítulo, exercícios/checklists, resumo final."
    validation: "cada capítulo entrega 1 micro-resultado"
  - id: s2
    description: "Rodar WLT-ebook-quality-checklist."
    validation: "PASS (objetivo, sem inchaço)"

postConditions:
  - { condition: "ebook blueprint gravado", errorMessage: "Blueprint de ebook incompleto." }

acceptanceCriteria:
  - "Consumível em <48h. Prático. Nada de livro inchado."

errorHandling:
  - { error: "ebook inchado", action: "cortar capítulos teóricos" }

handoff:
  next_agent: WLT-ai-production-prompt-engineer
  next_task: WLT-generate-ebook-generation-prompt
  condition: "WLT-ebook-quality-checklist PASS e usuário escolhe produzir"
  alternatives: ["continuar -> WLT-write-sales-page"]
