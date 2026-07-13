---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-order-bump-checklist]
templates: [WLT-order-bumps-template]
---

task:
  name: validateOrderBumps()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: product.order_bumps, tipo: list, origem: state, obrigatorio: true, validacao: "≥1 (ou justificativa)", default: null }
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: product.order_bumps_validation_status, tipo: enum(pass|fail|bypass), destino: state.product.order_bumps_validation_status, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Roda o checklist bump a bump." }
  interactive: { enabled: true, prompts: "Item a item com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "order_bumps existem no state", errorMessage: "Rode WLT-build-order-bumps antes." }

steps:
  - id: s1
    description: "Rodar WLT-order-bump-checklist em CADA bump (completa? não compete? marca-sem-pensar? valor imediato? entrega simples? 1 frase? nome autoexplicativo? não vira bagunça?)."
    validation: "todos os bumps avaliados"
    onFailure: "FAIL -> volta para WLT-build-order-bumps com o que refazer."
  - id: s2
    description: "Gravar order_bumps_validation_status."
    validation: "status gravado"

postConditions:
  - { condition: "order_bumps_validation_status ∈ {pass, fail, bypass}", errorMessage: "Status inválido." }

acceptanceCriteria:
  - "PASS só se TODOS os bumps mantidos passam no checklist. Bump reprovado sai ou volta pra refazer."

errorHandling:
  - { error: "usuário quer pular validação", action: "bypass só com frase exata; grava em journey.bypasses" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-offer-promise
  condition: "WLT-order-bump-checklist PASS (ou BYPASS documentado)"
  user_message: "Bumps travados. Agora a oferta completa — principal + bumps + upsell é onde o lucro mora."
  alternatives: ["FAIL -> WLT-build-order-bumps"]
