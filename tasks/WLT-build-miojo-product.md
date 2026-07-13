---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-miojo-product-checklist]
templates: [WLT-product-brief-template]
---

task:
  name: buildMiojoProduct()
  responsavel: WLT-product-miojo-builder
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: avatar.selected_sapato_apertado, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: demand_report, tipo: artifact, origem: state, obrigatorio: true, validacao: "decision=go", default: null }

outputs:
  - { campo: product_brief, tipo: artifact, destino: state.product.product_brief, persistido: true }
  - { campo: product.name, tipo: string, destino: state.product.name, persistido: true }
  - { campo: product.type, tipo: string, destino: state.product.type, persistido: true }
  - { campo: product.price, tipo: number, destino: state.product.price, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera product brief completo + nome." }
  interactive: { enabled: true, prompts: "Confirma formato e preço." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "sapato + dossier + demanda(go) existem", errorMessage: "Faltam pré-requisitos de produto." }

steps:
  - id: s1
    description: "Escolher formato (data/WLT-product-types.yaml) que entrega o quick win no menor atrito."
    validation: "formato justificado"
  - id: s2
    description: "Definir promessa, quick win, consumo (<48h), entregáveis, preço R$37-97."
    validation: "critérios miojo atendidos"
  - id: s3
    description: "Nomear (Gancho + Conexão + Palavra-chave + Fechamento). Evitar nome criativo/genérico."
    validation: "nome autoexplicativo em 3s"
  - id: s4
    description: "Preencher WLT-product-brief-template e gravar."
    validation: "brief completo"

postConditions:
  - { condition: "product_brief gravado", errorMessage: "Brief incompleto." }

acceptanceCriteria:
  - "Produto resolve UMA dor, <48h, sem curva, resultado tangível."
  - "'Por que é miojo' e 'por que vai vender' preenchidos."

errorHandling:
  - { error: "produto vira curso inchado", action: "recortar até virar miojo" }

handoff:
  next_agent: WLT-product-miojo-builder
  next_task: WLT-validate-miojo-product
  condition: "brief completo"
  alternatives: []
