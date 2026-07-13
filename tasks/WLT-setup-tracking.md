---
tools: [read_state, write_state, read_data, read_checklist]
checklists: [WLT-tracking-readiness-checklist]
templates: [WLT-traffic-plan-template]
---

task:
  name: setupTracking()
  responsavel: WLT-traffic-planner
  responsavel_type: Agente
  atomic_layer: Organism

inputs:
  - { campo: sales_page, tipo: artifact, origem: state, obrigatorio: true, validacao: "page.validation_status=pass", default: null }
  - { campo: product.price, tipo: number, origem: state, obrigatorio: true, validacao: ">0", default: null }
  - { campo: checkout_platform, tipo: string, origem: usuario, obrigatorio: true, validacao: "plataforma informada (ex: hotmart, kiwify)", default: null }

outputs:
  - { campo: traffic.tracking, tipo: object, destino: state.traffic.tracking, persistido: true }

executionModes:
  yolo: { enabled: true, prompts: "Monta o checklist de tracking completo e valida o que o usuário confirmar." }
  interactive: { enabled: true, prompts: "Pergunta o que já existe (Pixel? CAPI? evento de compra?) e conduz só o que falta." }
  preflight: { enabled: true }
  default: interactive

preConditions:
  - { condition: "página validada no state", errorMessage: "Tracking se instala na página publicada. Valide a página antes." }

steps:
  - id: s1
    description: "Infra (existe-no-metodo mod.10): BM com 2+ admins, conta de anúncios (BRL/SP), Página+Instagram conectados, Pixel criado. Ver data/WLT-tracking-stack.yaml -> infraestrutura/pixel."
    validation: "infra confirmada pelo usuário"
  - id: s2
    description: "Instalação: Pixel na landing (código base via prompt de produção) e no checkout (ID/token na plataforma). Ver tracking-stack -> pixel."
    validation: "Pixel instalado nos dois pontos"
  - id: s3
    description: "CAPI ativa + evento de compra como otimização (existe-no-metodo). Setup técnico da CAPI segue doc oficial (documentacao-oficial-verificar) — não improvisar."
    validation: "CAPI + Purchase confirmados"
  - id: s4
    description: "UTMs/nomenclatura (extensao): convenção do tracking-stack -> utms, alinhada à nomenclatura de campanha (produto+tipo+criativo)."
    validation: "convenção registrada no state"
  - id: s5
    description: "Validação: Pixel Helper verde, evento de teste/compra registrado, colunas de análise configuradas (inclusive personalizadas). Rodar WLT-tracking-readiness-checklist."
    validation: "WLT-tracking-readiness-checklist PASS"

postConditions:
  - { condition: "traffic.tracking gravado com validation=pass", errorMessage: "Tracking não validado — tráfego bloqueado." }

acceptanceCriteria:
  - "Pixel + CAPI + Purchase ativos ANTES da campanha (Playbook P10 passo 4)."
  - "Se o usuário já tem tudo configurado: verificar rapidamente (Pixel Helper + evento recente) e dar PASS sem retrabalho."

errorHandling:
  - { error: "usuário sem acesso à BM/checkout", action: "listar o que falta e pausar; não subir campanha sem tracking" }
  - { error: "Pixel não dispara na landing", action: "revisar inserção do código via prompt de produção; revalidar com Pixel Helper" }

handoff:
  next_agent: WLT-traffic-planner
  next_task: WLT-build-traffic-test-plan
  condition: "WLT-tracking-readiness-checklist PASS"
  alternatives: ["bypass só com a frase exata 'quero pular com risco documentado'"]
