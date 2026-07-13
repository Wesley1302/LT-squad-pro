---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-offer-obviousness-checklist]
templates: [WLT-offer-doc-template]
---

task:
  name: validateOffer()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "stack completo", default: null }

outputs:
  - { campo: offer.validation_status, tipo: enum(pass|fail|bypass), destino: state.offer.validation_status, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Roda o checklist de obviedade e decide." }
  interactive: { enabled: true, prompts: "Mostra item a item." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "offer stack completo", errorMessage: "Complete WLT-build-offer-stack antes." }

steps:
  - id: s1
    description: "Rodar WLT-offer-obviousness-checklist (promessa específica, prazo, objeção removida, big idea, mecanismo crível, entregáveis, prova, CTA, ancoragem, oferta óbvia)."
    validation: "todos avaliados"
    onFailure: "FAIL -> volta para o bloco de oferta correspondente."
  - id: s2
    description: "Gravar validation_status."
    validation: "gravado"

postConditions:
  - { condition: "validation_status definido", errorMessage: "Status inválido." }

acceptanceCriteria:
  - "PASS só se a oferta parece óbvia demais pra recusar."

errorHandling:
  - { error: "oferta não parece óbvia", action: "reforçar ancoragem/bônus/garantia" }

handoff:
  next_agent: WLT-deliverable-builder
  next_task: WLT-create-deliverables-blueprint
  condition: "WLT-offer-obviousness-checklist PASS (ou BYPASS)"
  alternatives: ["FAIL -> WLT-offer-architect"]
