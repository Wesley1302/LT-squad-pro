---
tools: [ads_library_lookup, web_fetch, read_state, write_state, read_checklist]
checklists: [WLT-creative-quality-checklist]
templates: [WLT-creative-matrix-template]
---

task:
  name: analyzeCreativeReferences()
  responsavel: WLT-creative-intelligence-analyst
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: reference_links, tipo: list, origem: usuario, obrigatorio: false, validacao: "-", default: [] }

outputs:
  - { campo: creatives.research, tipo: artifact, destino: state.creatives.research, persistido: true }
  - { campo: creatives.angles, tipo: list, destino: state.creatives.angles, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Garimpa referências e extrai anatomia + ângulos." }
  interactive: { enabled: true, prompts: "Pede links de referência." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "página validada (page.validation_status=pass)", errorMessage: "Faça a página antes dos criativos." }

steps:
  - id: s1
    description: "Coletar referências (Ads Library/TikTok/Reels/Shorts) e sinais de 'está vendendo' (variações, dias no ar)."
    actions: ["Sem web: pedir links ao usuário."]
    validation: "referências com links"
  - id: s2
    description: "Desmontar anatomia (hook/body/prova/CTA/formato/ângulo) — sem copiar conteúdo."
    validation: "anatomia extraída"
  - id: s3
    description: "Listar ângulos testáveis + oportunidades. Rodar WLT-creative-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "research + angles gravados", errorMessage: "Research incompleta." }

acceptanceCriteria:
  - "Padrões vencedores + ângulos, com links. Só anatomia, nunca cópia de conteúdo."

errorHandling:
  - { error: "sem referências", action: "trabalhar com o método + ângulos do avatar" }

handoff:
  next_agent: WLT-video-ads-builder
  next_task: WLT-generate-video-ads
  condition: "WLT-creative-quality-checklist PASS"
  alternatives: ["static -> WLT-generate-static-ads"]
