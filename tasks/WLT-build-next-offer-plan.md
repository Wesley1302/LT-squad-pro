---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-scale-readiness-checklist]
templates: [WLT-scale-plan-template]
---

task:
  name: buildNextOfferPlan()
  responsavel: WLT-scale-architect
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: scale_plan, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: funnel_economics, tipo: object, origem: state, obrigatorio: false, validacao: "bump/upsell conv, ticket médio (data/WLT-funnel-economics.yaml)", default: null }

outputs:
  - { campo: scale.next_offer, tipo: object, destino: state.scale.next_offer, persistido: true }
  - { campo: scale.roadmap_30_60_90, tipo: object, destino: state.scale.roadmap_30_60_90, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Decide próximo produto/upsell/bump/esteira + roadmap 30/60/90." }
  interactive: { enabled: true, prompts: "Confirma a direção da esteira." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "scale_plan existe", errorMessage: "Rode WLT-build-scale-plan antes." }

steps:
  - id: s1
    description: "Escolher o próximo movimento: variação de winner, novo bump/upsell, DOWNSELL (mod.14), novo produto, esteira, remarketing, novo ângulo/público. Basear em funnel_economics (ticket médio do funil, conversão de bump/upsell) e LTV por criativo (existe-no-metodo: cruzar criativo de origem -> compras posteriores)."
    validation: "decisão justificada por funnel_economics + LTV, não por achismo"
  - id: s2
    description: "Montar roadmap 30/60/90. Nova oferta segue o método (volta ao WLT-offer-architect/WLT-product-miojo-builder)."
    validation: "roadmap definido"

postConditions:
  - { condition: "next_offer + roadmap gravados", errorMessage: "Plano de próxima oferta incompleto." }

acceptanceCriteria:
  - "Decisão baseada em ROAS/LTV, não em achismo. Novo produto reinicia o ciclo pelo método."

errorHandling:
  - { error: "querer novo produto sem dado", action: "usar diagnóstico e LTV para decidir" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-offer-promise
  condition: "usuário decide criar nova oferta"
  alternatives: ["novo produto -> WLT-product-miojo-builder / WLT-build-miojo-product"]
