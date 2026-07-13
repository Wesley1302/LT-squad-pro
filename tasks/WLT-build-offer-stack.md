---
tools: [read_state, write_state, read_data]
checklists: [WLT-offer-obviousness-checklist]
templates: [WLT-offer-doc-template]
---

task:
  name: buildOfferStack()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: offer.mechanism, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: product.price, tipo: number, origem: state, obrigatorio: true, validacao: "37-147", default: null }

outputs:
  - { campo: offer.stack, tipo: list, destino: state.offer.stack, persistido: true }
  - { campo: offer.bonuses, tipo: list, destino: state.offer.bonuses, persistido: true }
  - { campo: product.order_bumps, tipo: list, destino: state.product.order_bumps, persistido: true }
  - { campo: product.upsell, tipo: object, destino: state.product.upsell, persistido: true }
  - { campo: offer.anchor, tipo: string, destino: state.offer.anchor, persistido: true }
  - { campo: offer.cta, tipo: string, destino: state.offer.cta, persistido: true }
  - { campo: offer.guarantee, tipo: string, destino: state.offer.guarantee, persistido: true }
  - { campo: offer.objections, tipo: list, destino: state.offer.objections, persistido: true }
  - { campo: offer.proof, tipo: string, destino: state.offer.proof, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Monta stack completo com bumps, upsell, ancoragem, CTA, garantia, objeções, prova, filtro." }
  interactive: { enabled: true, prompts: "Confirma preços de bump (~1/3) e upsell (~3x)." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "mechanism + price existem", errorMessage: "Complete promessa/big idea/mecanismo antes." }

steps:
  - id: s1
    description: "Listar entregáveis (ícone+nome+descrição, quantidades) + bônus separados."
    validation: "entregáveis concretos"
  - id: s2
    description: "Incorporar os order bump(s) JÁ VALIDADOS (state.product.order_bumps, gate WLT-order-bump-checklist). Não recriar — só integrar ao stack e à ancoragem."
    validation: "bump completa, não compete"
  - id: s3
    description: "Definir upsell (~3x, pós-compra), ancoragem (preços riscados + mockup tudão)."
    validation: "upsell + ancoragem"
  - id: s4
    description: "CTA verbal ('Quero acessar agora'), garantia, objeções + respostas, prova (real ou plano ético), filtro de comprador errado."
    validation: "todos definidos; nenhuma prova inventada"
  - id: s5
    description: "Gravar stack + calcular ticket máximo."
    validation: "stack completo"

postConditions:
  - { condition: "stack completo gravado", errorMessage: "Stack incompleto." }

acceptanceCriteria:
  - "Bump completa o próximo passo do mesmo problema."
  - "CTA verbal, nunca 'Comprar'. Prova real ou plano ético — nunca inventada."
  - "Ticket máximo calculado (principal + bumps + upsell)."

errorHandling:
  - { error: "bump compete com principal", action: "trocar por ângulo que completa" }
  - { error: "sem prova real", action: "montar plano ético de obtenção" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-validate-offer
  condition: "stack completo"
  alternatives: []
