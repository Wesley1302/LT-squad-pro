---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-scale-readiness-checklist]
templates: [WLT-scale-plan-template]
---

task:
  name: buildScalePlan()
  responsavel: WLT-scale-architect
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: diagnostic_report, tipo: artifact, origem: state, obrigatorio: true, validacao: "winner validado", default: null }
  - { campo: product.price, tipo: number, origem: state, obrigatorio: true, validacao: ">0", default: null }

outputs:
  - { campo: scale_plan, tipo: artifact, destino: state.scale.scale_plan, persistido: true }
  - { campo: scale.readiness, tipo: enum(not_ready|ready), destino: state.scale.readiness, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Monta plano de escala com estratégia e orçamento." }
  interactive: { enabled: true, prompts: "Confirma caixa disponível." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "winner validado (ROAS 2+, 50+ conversões) e estrutura ROAS 2", errorMessage: "Sem winner validado não se escala." }

steps:
  - id: s1
    description: "Rodar WLT-scale-readiness-checklist (ROAS 2+, CAC ok, winners, página validada, volume, LTV compreendido)."
    validation: "PASS (senão not_ready -> volta ao diagnóstico)"
  - id: s2
    description: "Escolher estratégia (10X/1-1-1/volume/horizontal), orçamento = 10x ticket, regra dia 1-2 do método (sem venda -> pausa; com venda e ROAS baixo -> +24h olhando histórico; reduzir orçamento antes de pausar; média >= ~1,5 conforme esteira)."
    validation: "plano definido"

postConditions:
  - { condition: "scale_plan gravado + readiness", errorMessage: "Plano incompleto." }

acceptanceCriteria:
  - "Escala só com winner validado. Regra dia-2 explícita."

errorHandling:
  - { error: "not_ready", action: "voltar ao WLT-funnel-diagnostician" }

handoff:
  next_agent: WLT-scale-architect
  next_task: WLT-build-next-offer-plan
  condition: "WLT-scale-readiness-checklist PASS"
  alternatives: ["not_ready -> WLT-funnel-diagnostician"]
