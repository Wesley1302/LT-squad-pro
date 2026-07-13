---
tools: [read_state, write_state, read_data]
checklists: [WLT-offer-obviousness-checklist]
templates: [WLT-offer-doc-template]
---

task:
  name: buildOfferMechanism()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: offer.big_idea, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: offer.mechanism, tipo: string, destino: state.offer.mechanism, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Descreve o mecanismo (o 'como' crível e único)." }
  interactive: { enabled: true, prompts: "Confirma o mecanismo com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "big_idea existe", errorMessage: "Rode WLT-build-offer-big-idea antes." }

steps:
  - id: s1
    description: "Descrever o mecanismo: por que ESTE método entrega o resultado (crível, simples)."
    validation: "mecanismo explicável em poucos passos"
  - id: s2
    description: "Gravar mechanism."
    validation: "gravado"

postConditions:
  - { condition: "mechanism gravado", errorMessage: "Mecanismo vazio." }

acceptanceCriteria:
  - "Mecanismo crível e diferenciado."
  - "Sem prometer curva de aprendizagem (mantém miojo)."

errorHandling:
  - { error: "mecanismo complexo demais", action: "simplificar para caber no miojo" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-offer-stack
  condition: "mechanism gravado"
  alternatives: []
