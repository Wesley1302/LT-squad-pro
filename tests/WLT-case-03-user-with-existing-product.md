# Test 03 — Usuário tem produto

**Input:** "Tenho um ebook de R$67 sobre organização financeira."

**Esperado:**
- `user_stage=has_product`, workflow `WLT-lt-with-existing-product`.
- Audita o produto pelo `WLT-miojo-product-checklist` (`WLT-validate-miojo-product`).
- Se FRACO (ex: curso fatiado / não resolve uma única dor) → refaz (`WLT-build-miojo-product`).
- Se APROVADO → segue para oferta (`WLT-build-offer-promise`).
- Se avatar/demanda não estão no state → volta para WLT-research-avatar antes de refazer.

**Falha se:** pular a auditoria; aprovar produto que falha o checklist.
