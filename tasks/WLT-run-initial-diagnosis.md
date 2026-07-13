---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-initial-diagnosis-checklist, WLT-anti-generic-output-checklist]
templates: [WLT-journey-state-template]
---

task:
  name: runInitialDiagnosis()
  responsavel: WLT-lt-chief
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - campo: user_message
    tipo: string
    origem: usuario
    obrigatorio: true
    validacao: "não vazio"
    default: null

outputs:
  - campo: journey.user_stage
    tipo: enum(from_zero|has_niche|has_product|has_traffic_no_sales|has_sales_wants_scale)
    destino: state.journey.user_stage
    persistido: true
  - campo: journey.current_workflow
    tipo: string
    destino: state.journey.current_workflow
    persistido: true

executionModes:
  yolo: { enabled: true, prompts: "Infere o stage pela mensagem e escolhe o workflow." }
  interactive: { enabled: true, prompts: "Faz até 3 perguntas cirúrgicas se o stage for ambíguo." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - condition: "state existe (mesmo zerado)"
    errorMessage: "Sem state. Inicialize a partir de templates/WLT-journey-state-template.yaml."

steps:
  - id: s1
    description: "Ler a mensagem e mapear para user_stage."
    actions:
      - "Classificar: não sei vender -> from_zero | tenho nicho -> has_niche | tenho produto -> has_product | gastei e não vendi -> has_traffic_no_sales | vendo e quero escalar -> has_sales_wants_scale."
    validation: "user_stage definido ou pergunta feita"
    onFailure: "Perguntar 1 coisa: 'Em uma frase, onde você está?'"
  - id: s2
    description: "Selecionar workflow por stage (config.activation.workflows)."
    actions: ["from_zero->WLT-lt-from-zero", "has_niche->WLT-lt-with-existing-niche", "has_product->WLT-lt-with-existing-product", "has_traffic_no_sales->WLT-funnel-diagnosis-cycle", "has_sales_wants_scale->WLT-scale-cycle"]
    validation: "workflow escolhido"
  - id: s3
    description: "Rodar WLT-initial-diagnosis-checklist e gravar state."
    validation: "checklist PASS"
    onFailure: "Pedir o dado ausente."

postConditions:
  - condition: "user_stage e current_workflow gravados"
    errorMessage: "Falha ao gravar diagnóstico no state."

acceptanceCriteria:
  - "Diagnóstico em ≤5 linhas."
  - "Nenhuma etapa avançada liberada sem pré-requisito."
  - "Resposta específica, não genérica (WLT-anti-generic-output-checklist)."

errorHandling:
  - { error: "mensagem ambígua", action: "1 pergunta cirúrgica" }
  - { error: "usuário pede etapa avançada", action: "bloquear: risco -> pré-req -> redireciona" }

handoff:
  next_agent: "por stage"
  next_task: "por workflow"
  condition: "WLT-initial-diagnosis-checklist PASS"
  alternatives:
    - "from_zero -> WLT-money-skill-strategist / WLT-identify-money-skill"
    - "has_niche -> WLT-avatar-researcher / WLT-research-avatar"
    - "has_product -> WLT-product-miojo-builder / WLT-validate-miojo-product (auditoria)"
    - "has_traffic_no_sales -> WLT-funnel-diagnostician / WLT-diagnose-funnel-bottleneck"
    - "has_sales_wants_scale -> WLT-scale-architect / WLT-build-scale-plan (checa prontidão)"
