---
tools: [web_search, web_fetch, comment_extractor, read_state, write_state, read_checklist]
checklists: [WLT-avatar-quality-checklist]
templates: [WLT-avatar-dossier-template]
---

task:
  name: researchAvatar()
  responsavel: WLT-avatar-researcher
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: identity.micro_niche, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: reference_material, tipo: list, origem: usuario, obrigatorio: false, validacao: "links/prints/transcrições", default: [] }

outputs:
  - { campo: avatar.profile, tipo: object, destino: state.avatar.profile, persistido: true }
  - { campo: avatar.pains_visible, tipo: list, destino: state.avatar.pains_visible, persistido: true }
  - { campo: avatar.pains_invisible, tipo: list, destino: state.avatar.pains_invisible, persistido: true }
  - { campo: avatar_dossier, tipo: artifact, destino: state.avatar, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Pesquisa e monta dossiê; marca sem-fonte como hipótese." }
  interactive: { enabled: true, prompts: "Pede links de vídeo +100k views e prints de comentários." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "micro_niche existe", errorMessage: "Sem micro-nicho. Rode WLT-compress-niche." }

steps:
  - id: s1
    description: "Coletar evidência das fontes (YouTube/Reddit/TikTok/Instagram/reviews/ads library) ou do material do usuário."
    actions: ["Sem web: acionar manual_research_protocol (tool-overrides.yaml)."]
    validation: "≥1 fonte real coletada"
    onFailure: "Pedir material específico ao usuário."
  - id: s2
    description: "Montar perfil + dores visíveis/invisíveis com citações reais (raw_quote)."
    validation: "dores separadas de hipóteses"
  - id: s3
    description: "Rodar WLT-avatar-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "avatar.profile + dores gravados com marcação evidência/hipótese", errorMessage: "Persona genérica não passa." }

acceptanceCriteria:
  - "Perfil com base em evidência real, não no 'Zé genérico'."
  - "Separação explícita evidência vs hipótese."

errorHandling:
  - { error: "sem acesso à web e sem material", action: "trabalhar só com o fornecido; sinalizar limitação" }

handoff:
  next_agent: WLT-avatar-researcher
  next_task: WLT-extract-voice-of-customer
  condition: "WLT-avatar-quality-checklist PASS"
  alternatives: []
