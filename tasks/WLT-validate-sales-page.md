---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-sales-page-12-dobras-checklist]
templates: [WLT-sales-page-template]
---

task:
  name: validateSalesPage()
  responsavel: WLT-sales-page-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: sales_page, tipo: artifact, origem: state, obrigatorio: true, validacao: "12 dobras", default: null }

outputs:
  - { campo: page.validation_status, tipo: enum(pass|fail|bypass), destino: state.page.validation_status, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Roda o checklist das 12 dobras." }
  interactive: { enabled: true, prompts: "Item a item." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "sales_page existe com 12 dobras", errorMessage: "Rode WLT-write-sales-page antes." }

steps:
  - id: s1
    description: "Rodar WLT-sales-page-12-dobras-checklist (inclui: preço só na dobra 9, CTA não 'Comprar', sem depoimento inventado)."
    validation: "todos avaliados"
    onFailure: "FAIL -> volta para WLT-write-sales-page."
  - id: s2
    description: "Gravar validation_status."
    validation: "gravado"

postConditions:
  - { condition: "validation_status definido", errorMessage: "Status inválido." }

acceptanceCriteria:
  - "PASS só com as 12 dobras corretas e as regras de preço/CTA/prova."

errorHandling:
  - { error: "dobra faltando/fora de ordem", action: "corrigir na WLT-write-sales-page" }

handoff:
  next_agent: WLT-creative-intelligence-analyst
  next_task: WLT-analyze-creative-references
  condition: "WLT-sales-page-12-dobras-checklist PASS (ou BYPASS)"
  alternatives: ["gerar prompt de landing -> WLT-ai-production-prompt-engineer"]
