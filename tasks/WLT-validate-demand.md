---
tools: [web_search, ads_library_lookup, read_state, write_state, read_checklist]
checklists: [WLT-demand-validation-checklist]
templates: [WLT-demand-report-template]
---

task:
  name: validateDemand()
  responsavel: WLT-demand-validator
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: avatar.sapatos_apertados, tipo: list, origem: state, obrigatorio: true, validacao: "≥1", default: null }
  - { campo: identity.micro_niche, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: demand_report, tipo: artifact, destino: state.demand, persistido: true }
  - { campo: demand.decision, tipo: enum(go|no_go|pivot_angle), destino: state.demand.decision, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Pontua scores e decide GO/NO_GO com base nas fontes." }
  interactive: { enabled: true, prompts: "Confirma sinais de mercado com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "sapatos_apertados existem", errorMessage: "Sem sapato. Rode WLT-extract-voice-of-customer." }

steps:
  - id: s1
    description: "Coletar sinais: procura, volume de comentários, views, concorrentes, anúncios ativos, urgência."
    actions: ["Sem web: pedir evidências ao usuário."]
    validation: "fontes registradas em demand.research_sources"
  - id: s2
    description: "Pontuar (0-10): demand, urgency, buying_intent, competition, creative_potential."
    validation: "5 scores"
  - id: s3
    description: "Decidir GO | NO_GO | PIVOT_ANGLE + riscos."
    validation: "WLT-demand-validation-checklist PASS"

postConditions:
  - { condition: "decisão + scores gravados com fontes", errorMessage: "Decisão sem evidência." }

acceptanceCriteria:
  - "5 scores com fonte. Decisão explícita."
  - "'Tem no YouTube grátis' não invalida (paga-se por atalho/conveniência)."

errorHandling:
  - { error: "sem sinal de demanda", action: "NO_GO ou PIVOT_ANGLE; devolver ao WLT-avatar-researcher" }

handoff:
  next_agent: WLT-avatar-researcher
  next_task: WLT-select-sapato-apertado
  condition: "WLT-demand-validation-checklist PASS e decisão = GO"
  alternatives: ["NO_GO/PIVOT -> WLT-avatar-researcher testa outro ângulo"]
