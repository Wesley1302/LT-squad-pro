# Contratos de Artefato — LT-squad-pro

Toda task gera um artefato; todo artefato alimenta a próxima task. Template = contrato de forma.

| Artefato | Produzido por | Template | Consumido por |
|---|---|---|---|
| journey_state | WLT-lt-chief | WLT-journey-state-template | todos |
| avatar_dossier | WLT-avatar-researcher | WLT-avatar-dossier-template | WLT-demand-validator, product, offer, page, criativos |
| evidence_ledger | WLT-avatar-researcher | WLT-evidence-ledger-template | avatar-quality/demanda |
| demand_report | WLT-demand-validator | WLT-demand-report-template | WLT-product-miojo-builder |
| product_brief | WLT-product-miojo-builder | WLT-product-brief-template | WLT-offer-architect, WLT-deliverable-builder |
| offer_doc | WLT-offer-architect | WLT-offer-doc-template | WLT-sales-page-builder, WLT-deliverable-builder, ai-production |
| deliverable_blueprint | WLT-deliverable-builder | WLT-deliverable-blueprint-template | WLT-sales-page-builder, ai-production |
| ebook_blueprint | WLT-deliverable-builder | WLT-ebook-blueprint-template | ai-production (ebook) |
| ai_agent_blueprint | WLT-deliverable-builder | WLT-ai-agent-blueprint-template | ai-production (agente) |
| sales_page | WLT-sales-page-builder | WLT-sales-page-template | WLT-creative-intelligence-analyst, ai-production (landing) |
| creative_matrix | video/WLT-static-ads-builder | WLT-creative-matrix-template | WLT-recording-director, WLT-traffic-planner |
| video_ads_round | WLT-video-ads-builder | WLT-video-ads-template | WLT-recording-director |
| static_ads_round | WLT-static-ads-builder | WLT-static-ads-template | WLT-recording-director |
| recording_plan | WLT-recording-director | WLT-recording-plan-template | WLT-traffic-planner |
| traffic_plan | WLT-traffic-planner | WLT-traffic-plan-template | WLT-funnel-diagnostician |
| diagnostic_report | WLT-funnel-diagnostician | WLT-diagnostic-report-template | WLT-scale-architect / donos de correção |
| scale_plan | WLT-scale-architect | WLT-scale-plan-template | WLT-offer-architect / product (nova oferta) |
| ai_production_prompt | WLT-ai-production-prompt-engineer | *-prompt-template | usuário / plataforma alvo |

## Regras
- Sem artefato → sem handoff.
- Artefato reprovado no checklist → não avança e não vira prompt de produção.
- Persistência: cada template mapeia para um caminho em `state/WLT-lt-journey-state.yaml`.
- Hipótese nunca é apresentada como fato (Evidence Ledger).
