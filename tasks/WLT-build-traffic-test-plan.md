---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-traffic-readiness-checklist]
templates: [WLT-traffic-plan-template]
---

task:
  name: buildTrafficTestPlan()
  responsavel: WLT-traffic-planner
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: creatives.video_ads, tipo: list, origem: state, obrigatorio: true, validacao: "≥5", default: null }
  - { campo: sales_page, tipo: artifact, origem: state, obrigatorio: true, validacao: "page.validation_status=pass", default: null }
  - { campo: product.price, tipo: number, origem: state, obrigatorio: true, validacao: ">0", default: null }

outputs:
  - { campo: traffic_plan, tipo: artifact, destino: state.traffic, persistido: true }
  - { campo: traffic.budget, tipo: number, destino: state.traffic.budget, persistido: true }
  - { campo: traffic.campaign_structure, tipo: object, destino: state.traffic.campaign_structure, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Monta estrutura + orçamento + janela 1-3-7 + metas." }
  interactive: { enabled: true, prompts: "Confirma verba disponível." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "produto + página(pass) + ≥5 criativos", errorMessage: "Tráfego sem produto+página+criativo é dinheiro jogado fora." }

steps:
  - id: s1
    description: "Estrutura: 1 campanha CBO, 1 conjunto Advantage+ (abertão, só Brasil, sem segmentar idade/gênero), 5-20 criativos."
    validation: "estrutura correta"
  - id: s2
    description: "Orçamento = nº criativos × (ticket/2). Verba curta: dividir por 2, rodar mais dias."
    validation: "orçamento calculado"
  - id: s3
    description: "Metas: ROAS 2.0+, CPV ≤R$2,70, CPC ≤R$23, CAC < ticket. Janela 1-3-7 + regras por dia."
    validation: "WLT-traffic-readiness-checklist PASS"

postConditions:
  - { condition: "traffic_plan gravado com metas e regras", errorMessage: "Plano incompleto." }

acceptanceCriteria:
  - "Separa teste de escala. Metas definidas ANTES. Não mexe em tudo ao mesmo tempo."
  - "Dia 1 só observa; regras dia 3+ (baixar/manter/escalar)."

errorHandling:
  - { error: "usuário quer escalar já", action: "bloquear: escala só com winner validado" }

handoff:
  next_agent: WLT-funnel-diagnostician
  next_task: WLT-diagnose-funnel-bottleneck
  condition: "usuário rodou e trouxe métricas"
  alternatives: ["configurar IA de tráfego (Camada 1/2) documentado no plano"]
