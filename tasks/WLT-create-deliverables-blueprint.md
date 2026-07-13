---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-deliverable-quality-checklist]
templates: [WLT-deliverable-blueprint-template]
---

task:
  name: createDeliverablesBlueprint()
  responsavel: WLT-deliverable-builder
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "offer.validation_status=pass", default: null }
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: deliverable_blueprint, tipo: artifact, destino: state.deliverables.blueprint, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Escolhe formato(s) e gera blueprint por entregável." }
  interactive: { enabled: true, prompts: "Confirma formatos com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "offer.validation_status = pass", errorMessage: "Oferta precisa estar aprovada." }

steps:
  - id: s1
    description: "Mapear cada entregável do stack para um formato (data/WLT-product-types.yaml)."
    validation: "formato por entregável"
  - id: s2
    description: "Gerar blueprint (estrutura, escopo, critério de pronto) por entregável; despachar para a task de blueprint específica se ebook/minicurso/agente/microapp."
    validation: "blueprint completo"
  - id: s3
    description: "Rodar WLT-deliverable-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "deliverable_blueprint gravado", errorMessage: "Blueprint incompleto." }

acceptanceCriteria:
  - "Cada blueprint cabe no miojo (48h, sem curva)."
  - "Aponta qual task de prompt de produção usar."

errorHandling:
  - { error: "entregável estoura o miojo", action: "recortar escopo" }

handoff:
  next_agent: WLT-sales-page-builder
  next_task: WLT-write-sales-page
  condition: "WLT-deliverable-quality-checklist PASS"
  alternatives: ["formato específico -> generate-*-blueprint", "usuário quer produzir -> WLT-ai-production-prompt-engineer"]
