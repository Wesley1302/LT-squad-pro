---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist, WLT-ebook-quality-checklist]
templates: [WLT-ebook-generation-prompt-template]
---

task:
  name: generateEbookGenerationPrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Page

inputs:
  - { campo: deliverables.ebook, tipo: artifact, origem: state, obrigatorio: true, validacao: "WLT-ebook-quality-checklist=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: product_brief, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt para outra LLM escrever o ebook." }
  interactive: { enabled: true, prompts: "Confirma tom e estrutura." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "ebook blueprint aprovado (e product brief aprovado)", errorMessage: "Não gera ebook se o brief/blueprint falharam." }

steps:
  - id: s1
    description: "Preencher WLT-ebook-generation-prompt-template: produto, avatar, dor, promessa, tom, estrutura, sumário, objetivo de cada capítulo, exemplos, exercícios, checklists, resumo final, o que evitar, estilo, critérios de qualidade, saída em Markdown."
    validation: "objetivo e prático, sem inchaço"
  - id: s2
    description: "Rodar checklists."
    validation: "PASS"

postConditions:
  - { condition: "prompt de ebook gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Ebook low ticket objetivo, prático, consumível. Nada de livro inchado. Saída em Markdown."

errorHandling:
  - { error: "estrutura inchada", action: "cortar capítulos teóricos" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: ["produzir no Manus/Claude/Gemini"]
