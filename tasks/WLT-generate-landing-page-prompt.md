---
tools: [read_state, read_template, read_data, read_checklist]
checklists: [WLT-ai-production-prompt-quality-checklist, WLT-landing-page-implementation-checklist]
templates: [WLT-landing-page-prompt-template]
---

task:
  name: generateLandingPagePrompt()
  responsavel: WLT-ai-production-prompt-engineer
  responsavel_type: Agente
  atomic_layer: Page

inputs:
  - { campo: offer_doc, tipo: artifact, origem: state, obrigatorio: true, validacao: "offer.validation_status=pass", default: null }
  - { campo: sales_page, tipo: artifact, origem: state, obrigatorio: true, validacao: "page.validation_status=pass", default: null }
  - { campo: avatar_dossier, tipo: artifact, origem: state, obrigatorio: true, validacao: "existe", default: null }
  - { campo: target_platform, tipo: enum, origem: usuario, obrigatorio: false, validacao: "lovable|antigravity|claude-code|codex|cursor|v0|bolt", default: lovable }

outputs:
  - { campo: ai_production.prompts_generated, tipo: list, destino: state.ai_production.prompts_generated, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Gera prompt de landing page premium completo." }
  interactive: { enabled: true, prompts: "Confirma plataforma alvo e nível de animação." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "offer + sales_page aprovados (validation_status=pass)", errorMessage: "Não gera landing se a oferta ou a página falharam no checklist." }

steps:
  - id: s1
    description: "Montar bloco ESTRATÉGIA (produto, avatar, promessa, oferta, big idea, mecanismo, prova, objeções, 12 dobras, CTA, garantia, FAQ)."
    validation: "estratégia do state, não inventada"
  - id: s2
    description: "Montar UI/UX (premium, refined gradients, subtle glassmorphism, clean hierarchy, mobile-first, conversion-focused, mockups, cards, prova, ancoragem, CTA sticky opcional)."
    validation: "diretrizes de data/WLT-landing-page-tech-stack.yaml (usar skills frontend-design + ui-ux-pro-max antes; impeccable depois)"
  - id: s3
    description: "Montar ANIMAÇÕES (GSAP, ScrollTrigger, Three.js, scroll reveals, staggered entrances, hover, parallax, mobile-safe). Corrigir: GSAP, não GASP."
    validation: "animações mobile-safe"
  - id: s4
    description: "Montar ENGENHARIA (HTML/CSS/JS ou framework, componentes, responsividade, acessibilidade, performance, SEO básico, assets placeholders, deploy, README, validação visual)."
    validation: "engenharia completa"
  - id: s5
    description: "Aplicar RESTRIÇÕES (sem depoimento inventado, preço só na dobra 9, CTA 'Quero acessar agora', sem texto genérico, sem animação pesada em mobile). Rodar checklists."
    validation: "WLT-ai-production-prompt-quality-checklist + WLT-landing-page-implementation-checklist PASS"

postConditions:
  - { condition: "prompt de landing gravado", errorMessage: "Prompt não persistido." }

acceptanceCriteria:
  - "Prompt pronto pra colar na plataforma. Estratégia + UI/UX + Animações + Engenharia + Restrições."
  - "Nada contradiz o Offer Doc/Avatar. GSAP correto."

errorHandling:
  - { error: "oferta/página reprovadas", action: "recusar e devolver para WLT-offer-architect/WLT-sales-page-builder" }

handoff:
  next_agent: WLT-lt-chief
  next_task: null
  condition: "prompt entregue"
  alternatives: ["skills externas: frontend-design (direção visual) + ui-ux-pro-max (UX/UI) antes de gerar; impeccable (QA) após implementar"]
