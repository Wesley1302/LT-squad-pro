---
tools: [web_search, comment_extractor, read_state, write_state, read_checklist]
checklists: [WLT-evidence-ledger-checklist]
templates: [WLT-evidence-ledger-template, WLT-avatar-dossier-template]
---

task:
  name: extractVoiceOfCustomer()
  responsavel: WLT-avatar-researcher
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: avatar.profile, tipo: object, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: reference_material, tipo: list, origem: usuario, obrigatorio: false, validacao: "-", default: [] }

outputs:
  - { campo: avatar.voices_in_head, tipo: list, destino: state.avatar.voices_in_head, persistido: true }
  - { campo: avatar.sapatos_apertados, tipo: list, destino: state.avatar.sapatos_apertados, persistido: true }
  - { campo: avatar.language_bank, tipo: list, destino: state.avatar.language_bank, persistido: true }
  - { campo: avatar.evidence_ledger, tipo: list, destino: state.avatar.evidence_ledger, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Extrai vozes/sapatos/linguagem e popula o ledger." }
  interactive: { enabled: true, prompts: "Confirma citações e categorias com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "avatar.profile existe", errorMessage: "Rode WLT-research-avatar antes." }

steps:
  - id: s1
    description: "Extrair vozes na cabeça (dúvida/comparação/derrota) com raw_quote real."
    validation: "cada voz tem fonte ou é hipótese"
  - id: s2
    description: "Extrair sapatos apertados (incômodo específico, recorrente, com o qual dá pra viver mas não quer)."
    validation: "≥3 sapatos candidatos"
  - id: s3
    description: "Popular language_bank (10 palavras/expressões mais usadas)."
    validation: "banco com palavras reais"
  - id: s4
    description: "Registrar cada descoberta como evidence_item (template) com category e confidence."
    validation: "WLT-evidence-ledger-checklist PASS"

postConditions:
  - { condition: "ledger populado, sem hipótese apresentada como fato", errorMessage: "Ledger inválido." }

acceptanceCriteria:
  - "Vozes e sapatos com citações reais quando há evidência."
  - "language_bank com termos do público (não do expert)."

errorHandling:
  - { error: "citação sem fonte", action: "confidence: baixa + marcar hipótese" }

handoff:
  next_agent: WLT-demand-validator
  next_task: WLT-validate-demand
  condition: "WLT-evidence-ledger-checklist PASS"
  alternatives: []
