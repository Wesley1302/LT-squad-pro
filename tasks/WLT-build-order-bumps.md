---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-order-bump-checklist]
templates: [WLT-order-bumps-template]
---

task:
  name: buildOrderBumps()
  responsavel: WLT-offer-architect
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "product.validation_status=pass", default: null }
  - { campo: avatar.selected_sapato_apertado, tipo: string, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar.objections, tipo: list, origem: state, obrigatorio: true, validacao: "existe", default: [] }

outputs:
  - { campo: product.order_bumps, tipo: list, destino: state.product.order_bumps, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera até 3 bumps (análise/execução/expansão) com preço ~1/3." }
  interactive: { enabled: true, prompts: "Confirma ângulos e preços com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "product.validation_status = pass (ou bypass)", errorMessage: "Order bump não vem antes de produto validado como Miojo." }

steps:
  - id: s1
    description: "Mapear o próximo passo lógico do comprador e a próxima objeção (a partir de sapato + objections + product_brief)."
    validation: "próximo passo + próxima objeção nomeados"
  - id: s2
    description: "Criar até 3 bumps quando fizer sentido — 1 por ângulo (análise, execução, expansão). Cada um: nome autoexplicativo, promessa em 1 frase, entregável simples, preço ~1/3 do principal (contexto > regra cega)."
    validation: "cada bump COMPLETA (não compete), explicável em 1 frase"
  - id: s3
    description: "Gravar em state.product.order_bumps (formato WLT-order-bumps-template)."
    validation: "bumps gravados"

postConditions:
  - { condition: "≥1 bump gravado (ou justificativa documentada de por que nenhum faz sentido)", errorMessage: "Sem bumps e sem justificativa." }

acceptanceCriteria:
  - "Bump completa o próximo passo do MESMO problema."
  - "Preço marca-sem-pensar (~1/3). Nome autoexplicativo. Entrega simples."

errorHandling:
  - { error: "bump compete com o principal", action: "trocar por ângulo que completa" }
  - { error: "bump vira produto complexo", action: "recortar até entrega simples" }

handoff:
  next_agent: WLT-offer-architect
  next_task: WLT-validate-order-bumps
  condition: "bumps gravados"
  alternatives: []
