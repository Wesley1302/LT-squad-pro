# Test 11 — Criativos consideram oferta completa (bumps + upsell + retargeting)

**Input:** "Me dá os criativos." (state: produto, bumps, upsell e página validados)

**Esperado:**
- `WLT-generate-video-ads` / `WLT-generate-static-ads` geram ≥5 (ideal 10+) com `role_in_funnel` preenchido na matriz.
- Front-end (`front_end_main_product` / `front_end_offer_stack`): vende o PRODUTO PRINCIPAL alinhado à oferta completa; NÃO vende bump diretamente; pode preparar desejo/percepção de valor pra esteira; `offer_confusion_risk` avaliado.
- Retargeting (`retargeting_order_bump_angle` / `retargeting_upsell_angle`): ângulos de expansão, complemento, prova, bônus, próximo passo — quando fizer sentido.
- Cada criativo registra: main_product, related_order_bump, related_upsell, awareness_stage, hipótese, métrica primária.
- Gates: `WLT-video-ad-checklist` / `WLT-static-ad-checklist` (itens de role_in_funnel).

**Falha se:** criativo sem role_in_funnel; front-end vendendo bump direto; matriz sem produto/bump/upsell relacionados; zero ângulo de retargeting quando há bumps/upsell validados.
