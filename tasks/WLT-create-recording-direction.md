---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-recording-direction-checklist]
templates: [WLT-recording-plan-template]
---

task:
  name: createRecordingDirection()
  responsavel: WLT-recording-director
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: creatives.video_ads, tipo: list, origem: state, obrigatorio: true, validacao: "≥1", default: null }

outputs:
  - { campo: creatives.recording_plan, tipo: artifact, destino: state.creatives.recording_plan, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera plano de gravação por criativo (rosto/faceless/UGC)." }
  interactive: { enabled: true, prompts: "Confirma cenário e formato." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "video_ads existem", errorMessage: "Rode WLT-generate-video-ads antes." }

steps:
  - id: s1
    description: "Por criativo: cenário, roupa, enquadramento, luz, expressão, energia, ritmo, gestos, objetos, b-roll, texto na tela, cortes, legendas."
    validation: "lo-fi, executável em celular"
  - id: s2
    description: "Gerar versões: com rosto, faceless, UGC. Rodar WLT-recording-direction-checklist."
    validation: "PASS"

postConditions:
  - { condition: "recording_plan gravado", errorMessage: "Plano incompleto." }

acceptanceCriteria:
  - "Direção lo-fi, rápida (30-50min/5 criativos), com CTA apontando o botão."

errorHandling:
  - { error: "direção exige produção cara", action: "simplificar para lo-fi" }

handoff:
  next_agent: WLT-traffic-planner
  next_task: WLT-build-traffic-test-plan
  condition: "WLT-recording-direction-checklist PASS"
  alternatives: []
