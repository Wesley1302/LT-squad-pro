---
tools: [read_state, write_state, read_data]
checklists: [WLT-offer-obviousness-checklist]
templates: [WLT-offer-doc-template]
---

task:
  name: buildOfferBigIdea()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: offer.primary_promise, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: offer.big_idea, tipo: string, destino: state.offer.big_idea, persistido: true }
  - { campo: offer.negation, tipo: string, destino: state.offer.negation, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera a tese diferenciada + negação do jeito antigo." }
  interactive: { enabled: true, prompts: "Confirma o reposicionamento com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "primary_promise existe", errorMessage: "Rode WLT-build-offer-promise antes." }

steps:
  - id: s1
    description: "Formular a big idea: por que o jeito antigo falha e por que este reposiciona o problema."
    validation: "tese + negação"
  - id: s2
    description: "Gravar big_idea e negation."
    validation: "gravado"

postConditions:
  - { condition: "big_idea gravada", errorMessage: "Big idea vazia." }

acceptanceCriteria:
  - "Big idea diferenciada, não clichê."
  - "Negação clara do jeito antigo."

errorHandling:
  - { error: "big idea genérica", action: "buscar o ângulo não óbvio do avatar" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-build-offer-mechanism
  condition: "big_idea gravada"
  alternatives: []
