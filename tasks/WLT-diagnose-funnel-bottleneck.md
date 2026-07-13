---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-funnel-diagnosis-checklist]
templates: [WLT-diagnostic-report-template]
---

task:
  name: diagnoseFunnelBottleneck()
  responsavel: WLT-funnel-diagnostician
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: metrics, tipo: object, origem: usuario, obrigatorio: true, validacao: "roas/cpv/cpc/cac + secundárias", default: null }
  - { campo: retention_metrics, tipo: object, origem: usuario, obrigatorio: false, validacao: "view3s/view25/view50/tx_compra_view50", default: null }
  - { campo: frequency, tipo: number, origem: usuario, obrigatorio: false, validacao: ">=0", default: null }
  - { campo: funnel_economics, tipo: object, origem: state, obrigatorio: false, validacao: "bump_conversion_rate etc.", default: null }

outputs:
  - { campo: diagnostic_report, tipo: artifact, destino: state.traffic, persistido: true }
  - { campo: traffic.bottleneck, tipo: string, destino: state.traffic.bottleneck, persistido: true }
  - { campo: traffic.diagnosis, tipo: string, destino: state.traffic.diagnosis, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Isola o primeiro número quebrado e roteia a correção." }
  interactive: { enabled: true, prompts: "Pede as métricas que faltam." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "usuário forneceu métricas (janela válida: 7d teste / mês+semana+3d se convertendo)", errorMessage: "Sem métricas não há diagnóstico." }

steps:
  - id: s1
    description: "Ler R3C de cima pra baixo (CPV -> CPC -> CAC). Isolar o PRIMEIRO fora da meta."
    validation: "gargalo isolado"
  - id: s2
    description: "Mapear culpado (data/WLT-bottleneck-diagnosis-map.yaml): CPV->criativo/público; CPC->página; CAC->oferta/checkout."
    validation: "culpado + correção"
  - id: s3
    description: "Se ROAS<2 e usuário quer escalar: bloquear (é hora de corrigir rota). Rodar WLT-funnel-diagnosis-checklist."
    validation: "PASS"

postConditions:
  - { condition: "diagnostic_report gravado", errorMessage: "Diagnóstico incompleto." }

acceptanceCriteria:
  - "Aponta o número, não 'provavelmente'. Uma correção por vez."
  - "Não manda trocar produto sem teste válido (7d verba real)."

errorHandling:
  - { error: "zero venda após teste válido", action: "rotear pivot (produto/oferta/ângulo) — desapego" }

handoff:
  next_agent: WLT-scale-architect
  next_task: WLT-build-scale-plan
  condition: "winner validado (ROAS 2+, 50+ conversões)"
  alternatives:
    - "CPV alto -> WLT-creative-intelligence-analyst / WLT-video-ads-builder"
    - "CPC alto -> WLT-sales-page-builder"
    - "CAC alto -> WLT-offer-architect"
    - "sem venda válida -> WLT-product-miojo-builder (pivot)"
