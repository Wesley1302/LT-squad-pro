---
tools: [read_state, write_state, read_data]
checklists: [WLT-sales-page-12-dobras-checklist]
templates: [WLT-sales-page-template]
---

task:
  name: writeSalesPage()
  responsavel: WLT-sales-page-builder
  responsavel_type: Agente
  atomic_layer: Page

inputs:
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "offer.validation_status=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: sales_page, tipo: artifact, destino: state.page, persistido: true }
  - { campo: page.twelve_folds, tipo: list, destino: state.page.twelve_folds, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Escreve as 12 dobras completas." }
  interactive: { enabled: true, prompts: "Aprova dobra a dobra." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "offer.validation_status = pass", errorMessage: "Sem oferta aprovada, não há página." }

steps:
  - id: s1
    description: "Escrever as 12 dobras na ordem (data/WLT-page-12-dobras.yaml), preço só na dobra 9."
    validation: "12 dobras presentes, na ordem"
  - id: s2
    description: "Dobra 1 silencia ceticismo/rejeição/defesa em 3s. Copy usa language_bank."
    validation: "vozes silenciadas"
  - id: s3
    description: "Prova: real (print com nome) ou plano ético — nunca inventada. CTA verbal."
    validation: "sem depoimento inventado"

postConditions:
  - { condition: "sales_page gravada com 12 dobras", errorMessage: "Página incompleta." }

acceptanceCriteria:
  - "Preço só na dobra 9. CTA 'Quero acessar agora', nunca 'Comprar'."
  - "Continua a conversa do criativo, sem texto genérico."

errorHandling:
  - { error: "preço antes da dobra 9", action: "mover para a dobra 9" }
  - { error: "sem prova", action: "montar plano ético (não inventar)" }

handoff:
  next_agent: WLT-sales-page-builder
  next_task: WLT-validate-sales-page
  condition: "12 dobras escritas"
  alternatives: ["gerar página premium -> WLT-ai-production-prompt-engineer / WLT-generate-landing-page-prompt"]
