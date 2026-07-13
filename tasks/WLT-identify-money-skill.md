---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-money-skill-checklist]
templates: [WLT-journey-state-template]
---

task:
  name: identifyMoneySkill()
  responsavel: WLT-money-skill-strategist
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: user_background, tipo: string, origem: usuario, obrigatorio: true, validacao: "não vazio", default: null }

outputs:
  - { campo: identity.skills, tipo: list, destino: state.identity.skills, persistido: true }
  - { campo: identity.selected_skill, tipo: string, destino: state.identity.selected_skill, persistido: true }
  - { campo: identity.validation_notes, tipo: string, destino: state.identity.validation_notes, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Infere skills do background e seleciona a mais monetizável com evidência." }
  interactive: { enabled: true, prompts: "3-7 perguntas cirúrgicas sobre resultado real e validação externa." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "journey.user_stage = from_zero (ou skill não definida)", errorMessage: "Este passo é do fluxo do zero." }

steps:
  - id: s1
    description: "Listar habilidades candidatas com evidência de aplicação."
    actions: ["Perguntar: quem você já ajudou a conseguir o quê? já foi pago/elogiado por isso?"]
    validation: "≥1 skill com evidência ou marcada como hipótese"
  - id: s2
    description: "Filtrar com a lógica: conhecimento vs skill aplicada; repetição vence diploma; melhor que a média já vende; hobby não é produto."
    validation: "1 skill selecionada"
  - id: s3
    description: "Rodar WLT-money-skill-checklist e gravar."
    validation: "PASS"
    onFailure: "Pedir prova de resultado externo."

postConditions:
  - { condition: "selected_skill gravada com nota de validação", errorMessage: "Skill sem evidência não pode ser 'fato'." }

acceptanceCriteria:
  - "1 skill selecionada + por que monetiza + evidência (ou hipótese marcada)."
  - "Sem elogiar hobby. Sem aceitar conhecimento sem aplicação."

errorHandling:
  - { error: "nenhuma skill com evidência", action: "devolver ao WLT-lt-chief com pergunta específica" }

handoff:
  next_agent: WLT-niche-compressor
  next_task: WLT-compress-niche
  condition: "WLT-money-skill-checklist PASS"
  alternatives: ["sem evidência -> permanecer e pedir prova"]
