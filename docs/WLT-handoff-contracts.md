# Contratos de Handoff — LT-squad-pro

Cada handoff só dispara com `required_artifacts` presentes no state E o checklist em PASS (ou BYPASS documentado).
Mapa completo em `data/WLT-handoff-map.yaml`. Formato do contrato:

```yaml
handoff:
  from_agent:
  to_agent:
  from_task:
  to_task:
  required_artifacts:
  state_updates:
  pass_condition:
  fail_condition:
  user_message:
```

## Contratos principais

```yaml
handoff:
  from_agent: WLT-product-miojo-builder
  to_agent: WLT-offer-architect
  from_task: WLT-validate-miojo-product
  to_task: WLT-build-offer-promise
  required_artifacts: [product_brief, avatar_dossier, demand_report]
  state_updates: [state.product.validation_status]
  pass_condition: "Produto aprovado no checklist Miojo"
  fail_condition: "Produto não resolve uma única dor ou não é consumível em 48h"
  user_message: "Produto travado. Agora vamos construir a oferta. Sem oferta, produto é só arquivo bonito."
```
```yaml
handoff:
  from_agent: WLT-product-miojo-builder
  to_agent: WLT-offer-architect
  from_task: WLT-validate-miojo-product
  to_task: WLT-build-order-bumps
  required_artifacts: [product_brief, avatar_dossier, demand_report]
  state_updates: [state.product.validation_status]
  pass_condition: "WLT-miojo-product-checklist PASS"
  fail_condition: "Produto reprovado no checklist Miojo"
  user_message: "Produto travado. Agora os order bumps — ~30% do lucro mora neles."
```
```yaml
handoff:
  from_agent: WLT-offer-architect
  to_agent: WLT-offer-architect
  from_task: WLT-validate-order-bumps
  to_task: WLT-build-offer-promise
  required_artifacts: [product.order_bumps, product_brief]
  state_updates: [state.product.order_bumps_validation_status]
  pass_condition: "WLT-order-bump-checklist PASS (bump completa, não compete, marca-sem-pensar)"
  fail_condition: "Bump compete / preço exige pensar / entrega complexa"
  user_message: "Bumps travados. Agora a oferta completa — principal + bumps + upsell é onde o lucro mora."
```
```yaml
handoff:
  from_agent: WLT-offer-architect
  to_agent: WLT-sales-page-builder
  from_task: WLT-validate-offer
  to_task: WLT-write-sales-page
  required_artifacts: [offer_doc, avatar_dossier]
  state_updates: [state.offer.validation_status]
  pass_condition: "WLT-offer-obviousness-checklist PASS"
  fail_condition: "Oferta não parece óbvia / sem prova real ou plano ético"
  user_message: "Oferta óbvia pronta. Agora a página que continua a conversa do criativo."
```
```yaml
handoff:
  from_agent: WLT-sales-page-builder
  to_agent: WLT-creative-intelligence-analyst
  from_task: WLT-validate-sales-page
  to_task: WLT-analyze-creative-references
  required_artifacts: [sales_page, offer_doc, avatar_dossier]
  state_updates: [state.page.validation_status]
  pass_condition: "WLT-sales-page-12-dobras-checklist PASS"
  fail_condition: "Dobra faltando / preço antes da 9 / depoimento inventado"
  user_message: "Página no ar. Agora os criativos — o 80/20."
```
```yaml
handoff:
  from_agent: WLT-traffic-planner
  to_agent: WLT-funnel-diagnostician
  from_task: WLT-build-traffic-test-plan
  to_task: WLT-diagnose-funnel-bottleneck
  required_artifacts: [traffic_plan]
  state_updates: [state.traffic.*]
  pass_condition: "usuário rodou tráfego e trouxe métricas"
  fail_condition: "sem métricas"
  user_message: "Me manda as 4 métricas (ROAS/CPV/CPC/CAC) que eu te digo onde tá a ponta solta."
```
```yaml
handoff:
  from_agent: WLT-funnel-diagnostician
  to_agent: WLT-scale-architect
  from_task: WLT-diagnose-funnel-bottleneck
  to_task: WLT-build-scale-plan
  required_artifacts: [diagnostic_report]
  state_updates: [state.traffic.bottleneck]
  pass_condition: "winner validado (ROAS 2+, 50+ conversões)"
  fail_condition: "ROAS<2 sem LTV comprovado"
  user_message: "Winner validado. Agora escala com verba de 10x o ticket."
```

## AI Production (branch)
```yaml
handoff:
  from_agent: <qualquer agent dono de artefato aprovado>
  to_agent: WLT-ai-production-prompt-engineer
  from_task: <task da etapa aprovada>
  to_task: <generate-*-prompt por artefato>
  required_artifacts: [<artefato com validation_status=pass>]
  pass_condition: "artefato aprovado no checklist"
  fail_condition: "artefato reprovado -> recusa e devolve ao dono"
  user_message: "Prompt pronto pra <plataforma>. Cola e roda."
```
