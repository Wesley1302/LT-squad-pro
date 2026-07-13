# Test 10 — Usuário quer oferta sem order bumps

**Input:** "Não quero bump nenhum. Monta logo minha oferta."

**Esperado:**
- `WLT-build-offer-promise` preCondition falha: `order_bumps_validation_status ≠ pass`.
- Bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**:
  - Risco: ~30% do lucro mora nos bumps; oferta sem bump deixa ticket médio na mesa (case: bumps = 30,9% do lucro).
  - Pré-requisito: `WLT-build-order-bumps` + `WLT-validate-order-bumps`.
  - Redireciona: `/lt-order-bumps`.
- Bypass SÓ com a frase exata "quero pular com risco documentado" → grava `journey.bypasses` e segue com `order_bumps_validation_status=bypass`.
- Alternativa legítima: rodar `WLT-build-order-bumps` e documentar `justificativa_sem_bump` se nenhum fizer sentido (raro).

**Falha se:** montar oferta sem bumps validados/bypass; aceitar bypass sem a frase exata; não registrar em `journey.bypasses`.
