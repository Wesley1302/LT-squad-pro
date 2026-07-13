---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-niche-compression-checklist]
templates: [WLT-journey-state-template]
---

task:
  name: compressNiche()
  responsavel: WLT-niche-compressor
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: identity.selected_skill, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: raw_niche, tipo: string, origem: usuario, obrigatorio: false, validacao: "-", default: null }

outputs:
  - { campo: identity.niche, tipo: string, destino: state.identity.niche, persistido: true }
  - { campo: identity.micro_niche, tipo: string, destino: state.identity.micro_niche, persistido: true }
  - { campo: identity.unfair_advantage, tipo: string, destino: state.identity.unfair_advantage, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Comprime direto para micro-nicho + vantagem injusta." }
  interactive: { enabled: true, prompts: "Pergunta público, contexto e resultado específicos." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "selected_skill existe", errorMessage: "Sem skill selecionada. Rode WLT-identify-money-skill." }

steps:
  - id: s1
    description: "Comprimir nicho amplo em micro-nicho: [público específico] que [contexto] e quer [resultado]."
    validation: "micro-nicho com os 3 elementos"
  - id: s2
    description: "Identificar vantagem injusta (o que o usuário tem que os concorrentes não têm)."
    validation: "1 vantagem nomeada"
  - id: s3
    description: "Rodar WLT-niche-compression-checklist e gravar."
    validation: "PASS"

postConditions:
  - { condition: "micro_niche + unfair_advantage gravados", errorMessage: "Micro-nicho incompleto." }

acceptanceCriteria:
  - "Micro-nicho em 1 frase (público + contexto + resultado)."
  - "Especificidade suficiente para preço maior."

errorHandling:
  - { error: "nicho ainda genérico", action: "comprimir mais um nível" }

handoff:
  next_agent: WLT-avatar-researcher
  next_task: WLT-research-avatar
  condition: "WLT-niche-compression-checklist PASS"
  alternatives: []
