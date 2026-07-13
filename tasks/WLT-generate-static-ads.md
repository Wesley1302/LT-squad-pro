---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-static-ad-checklist, WLT-creative-quality-checklist]
templates: [WLT-static-ads-template, WLT-creative-matrix-template]
---

task:
  name: generateStaticAds()
  responsavel: WLT-static-ads-builder
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: creatives.research, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: creatives.static_ads, tipo: list, destino: state.creatives.static_ads, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera ≥5 estáticos/carrosséis (ideal 10+) + prompts de imagem." }
  interactive: { enabled: true, prompts: "Confirma formatos." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "creatives.research existe", errorMessage: "Rode WLT-analyze-creative-references antes." }

steps:
  - id: s1
    description: "Gerar ≥5 estáticos/carrosséis (ideal 10+) (headline, print, comparação, checklist visual, mockup, tabela de erros, meme, advertorial)."
    validation: "≥5 (ideal 10+)"
  - id: s2
    description: "Headlines com language_bank; ancorar valor antes do clique; prompt de imagem por criativo (1:1/4:5/9:16); classificar role_in_funnel + main_product + related_order_bump/upsell + awareness_stage + offer_confusion_risk (front-end não vende bump direto; retargeting usa expansão/prova/próximo passo)."
    validation: "WLT-static-ad-checklist PASS"

postConditions:
  - { condition: "static_ads[] gravado (≥5)", errorMessage: "Menos de 5." }

acceptanceCriteria:
  - "≥5 estáticos (ideal 10+). Prova real, nunca fabricada. CTA leva pra página. role_in_funnel + produto/bump/upsell registrados na matriz."

errorHandling:
  - { error: "<5", action: "bloquear e gerar mais variações" }
  - { error: "5-9", action: "aviso: abaixo do ideal (10+), não bloqueia" }

handoff:
  next_agent: WLT-recording-director
  next_task: WLT-create-recording-direction
  condition: "WLT-static-ad-checklist PASS"
  alternatives: ["produzir imagem -> WLT-ai-production-prompt-engineer / WLT-generate-design-system-prompt"]
