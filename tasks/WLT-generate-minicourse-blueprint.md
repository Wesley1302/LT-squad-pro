---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-deliverable-quality-checklist]
templates: [WLT-deliverable-blueprint-template]
---

task:
  name: generateMinicourseBlueprint()
  responsavel: WLT-deliverable-builder
  responsavel_type: Agente
  atomic_layer: Template

inputs:
  - { campo: deliverable_blueprint, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: deliverables.minicourse, tipo: artifact, destino: state.deliverables.minicourse, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera módulos, aulas, duração, quick win e ordem de gravação." }
  interactive: { enabled: true, prompts: "Confirma módulos." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "deliverable_blueprint existe", errorMessage: "Rode WLT-create-deliverables-blueprint antes." }

steps:
  - id: s1
    description: "Definir módulos + aulas (máx 1-2h total), quick win na 1ª aula, roteiro por aula, exercícios, materiais de apoio, ordem de gravação."
    validation: "aluno aplica no mesmo dia"
  - id: s2
    description: "Rodar WLT-deliverable-quality-checklist."
    validation: "PASS"

postConditions:
  - { condition: "minicourse blueprint gravado", errorMessage: "Incompleto." }

acceptanceCriteria:
  - "Máx 1-2h de vídeo. Não é curso completo disfarçado."

errorHandling:
  - { error: "curso longo demais", action: "reduzir a 1-2h com quick win" }

handoff:
  next_agent: WLT-sales-page-builder
  next_task: WLT-write-sales-page
  condition: "WLT-deliverable-quality-checklist PASS"
  alternatives: ["produzir -> WLT-ai-production-prompt-engineer / WLT-generate-implementation-prompt"]
