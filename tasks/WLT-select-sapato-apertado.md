---
tools: [read_state, write_state, read_checklist]
checklists: [WLT-avatar-quality-checklist]
templates: [WLT-avatar-dossier-template]
---

task:
  name: selectSapatoApertado()
  responsavel: WLT-avatar-researcher
  responsavel_type: Agente
  atomic_layer: Molecule

inputs:
  - { campo: avatar.sapatos_apertados, tipo: list, origem: state, obrigatorio: true, validacao: "≥1", default: null }
  - { campo: demand.decision, tipo: enum, origem: state, obrigatorio: true, validacao: "go", default: null }

outputs:
  - { campo: avatar.selected_sapato_apertado, tipo: string, destino: state.avatar.selected_sapato_apertado, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Escolhe o sapato mais recorrente + doloroso + com potencial de produto/criativo." }
  interactive: { enabled: true, prompts: "Confirma a escolha com o usuário." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "demand.decision = go", errorMessage: "Valide a demanda antes de fixar o sapato." }

steps:
  - id: s1
    description: "Ranquear sapatos por recorrência, dor, potencial de produto instantâneo e potencial criativo."
    validation: "ranking justificado"
  - id: s2
    description: "Selecionar 1 e gravar."
    validation: "WLT-avatar-quality-checklist PASS"

postConditions:
  - { condition: "selected_sapato_apertado gravado", errorMessage: "Nenhum sapato selecionado." }

acceptanceCriteria:
  - "1 sapato selecionado, explicável em 1 frase pelo público."
  - "Não é dor da vida toda — é incômodo recorrente."

errorHandling:
  - { error: "sapato genérico demais", action: "voltar e especificar" }

handoff:
  next_agent: WLT-product-miojo-builder
  next_task: WLT-build-miojo-product
  condition: "WLT-avatar-quality-checklist PASS"
  alternatives: []
