---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-miojo-product-checklist]
templates: [WLT-product-brief-template]
---

task:
  name: validateMiojoProduct()
  responsavel: WLT-product-miojo-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: product.validation_status, tipo: enum(pass|fail|bypass), destino: state.product.validation_status, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Roda o checklist e decide." }
  interactive: { enabled: true, prompts: "Mostra item a item ao usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "product_brief existe", errorMessage: "Sem brief. Rode WLT-build-miojo-product (ou audite o produto existente)." }

steps:
  - id: s1
    description: "Rodar WLT-miojo-product-checklist item a item."
    validation: "todos os itens avaliados"
    onFailure: "FAIL -> volta para WLT-build-miojo-product com o que refazer."
  - id: s2
    description: "Gravar validation_status."
    validation: "status gravado"

postConditions:
  - { condition: "validation_status ∈ {pass, fail, bypass}", errorMessage: "Status inválido." }

acceptanceCriteria:
  - "PASS só se TODOS os itens do checklist passam."
  - "Curso caro fatiado = FAIL."

errorHandling:
  - { error: "usuário quer pular validação", action: "bypass só com frase exata; grava em journey.bypasses" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-order-bumps
  condition: "WLT-miojo-product-checklist PASS (ou BYPASS documentado)"
  user_message: "Produto travado. Agora os order bumps — ~30% do lucro mora neles. Depois a oferta completa."
  alternatives: ["FAIL -> WLT-product-miojo-builder / WLT-build-miojo-product"]
