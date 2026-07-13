# Test 09 — Produto validado, sem bumps

**Input:** "Meu produto passou no checklist Miojo. Qual o próximo passo?"

**Esperado:**
- `WLT-lt-chief` lê state: `product.validation_status=pass`, `order_bumps` vazio.
- Handoff para `WLT-offer-architect` / `WLT-build-order-bumps` (NÃO para a oferta).
- Mensagem: "Produto travado. Agora os order bumps — ~30% do lucro mora neles."
- Cria até 3 bumps (análise/execução/expansão) a partir de sapato + próxima objeção + próximo passo; preço ~1/3.
- Depois `WLT-validate-order-bumps` (gate `WLT-order-bump-checklist`).

**Falha se:** pular direto pra `WLT-build-offer-promise`; tratar bump como opcional; criar bump que compete com o principal.
