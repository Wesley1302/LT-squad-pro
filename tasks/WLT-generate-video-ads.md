---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-video-ad-checklist, WLT-creative-quality-checklist]
templates: [WLT-video-ads-template, WLT-creative-matrix-template]
---

task:
  name: generateVideoAds()
  responsavel: WLT-video-ads-builder
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: creatives.research, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: creatives.video_ads, tipo: list, destino: state.creatives.video_ads, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera ≥5 roteiros (ideal 10+) variados com hipótese e métrica cada." }
  interactive: { enabled: true, prompts: "Confirma formatos e ângulos." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "creatives.research existe", errorMessage: "Rode WLT-analyze-creative-references antes." }

steps:
  - id: s1
    description: "Gerar ≥5 roteiros (ideal 10+) misturando formatos (lo-fi, caixinha, fofoquinha, react, print, UGC...) e ganchos (8 mecanismos)."
    validation: "≥5 roteiros (ideal 10+)"
  - id: s2
    description: "Cada roteiro segue Fórmula A (sapato->voz->solução->CTA) ou B (APP)."
    validation: "estrutura correta"
  - id: s3
    description: "Classificar cada criativo na matriz: role_in_funnel + main_product + related_order_bump/upsell + awareness_stage + offer_confusion_risk. Front-end vende o principal (não o bump direto); retargeting usa ângulos de expansão/prova/próximo passo."
    validation: "role_in_funnel preenchido; nenhum front-end vendendo bump diretamente"
  - id: s4
    description: "Cada criativo carrega hipótese testável + métrica primária (view3s/25/50). Rodar WLT-video-ad-checklist."
    validation: "PASS"

postConditions:
  - { condition: "video_ads[] gravado (≥5)", errorMessage: "Menos de 5 criativos." }

acceptanceCriteria:
  - "≥5 criativos (ideal 10+). CTA aponta o botão. Não vende tudo no criativo. Sem prova inventada. role_in_funnel + produto/bump/upsell registrados na matriz."

errorHandling:
  - { error: "<5 criativos", action: "bloquear e gerar mais variações" }
  - { error: "5-9 criativos", action: "aviso: abaixo do ideal (10+), não bloqueia" }

handoff:
  next_agent: WLT-static-ads-builder
  next_task: WLT-generate-static-ads
  condition: "WLT-video-ad-checklist PASS"
  alternatives: ["direção -> WLT-create-recording-direction", "produzir -> WLT-ai-production-prompt-engineer"]
