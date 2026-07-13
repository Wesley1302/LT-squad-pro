---
tools: [read_state, write_state, read_data]
checklists: [WLT-offer-obviousness-checklist]
templates: [WLT-offer-doc-template]
---

task:
  name: buildOfferPromise()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "product.validation_status=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: offer.primary_promise, tipo: string, destino: state.offer.primary_promise, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera a promessa: resultado + prazo + sem [objeção]." }
  interactive: { enabled: true, prompts: "Confirma resultado e prazo com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "product.validation_status = pass (ou bypass)", errorMessage: "Oferta não vem antes de produto validado." }
  - { condition: "product.order_bumps_validation_status = pass (ou bypass)", errorMessage: "Oferta completa não vem antes de order bumps validados. Rode WLT-build-order-bumps." }

steps:
  - id: s1
    description: "Formular promessa no padrão: [resultado específico] em [prazo] sem [objeção principal]."
    validation: "promessa com os 3 elementos, na linguagem do avatar"
  - id: s2
    description: "Gravar em offer.primary_promise."
    validation: "gravado"

postConditions:
  - { condition: "primary_promise gravada", errorMessage: "Promessa vazia." }

acceptanceCriteria:
  - "Promessa específica, com prazo e objeção removida."
  - "Usa language_bank do avatar."

errorHandling:
  - { error: "promessa vaga", action: "especificar resultado e prazo" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-offer-big-idea
  condition: "promessa gravada"
  alternatives: []
