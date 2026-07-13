# Test 04 — Tráfego sem vendas

**Input:** "Gastei R$300 e não vendi nada."

**Esperado:**
- `user_stage=has_traffic_no_sales`, workflow `WLT-funnel-diagnosis-cycle`.
- `WLT-funnel-diagnostician` PEDE as 4 métricas (ROAS/CPV/CPC/CAC) + secundárias.
- Diagnostica o gargalo (CPV→criativo; CPC→página; CAC→oferta/checkout).
- NÃO manda trocar produto sem teste válido (7d verba real) e evidência.

**Falha se:** mandar trocar produto no susto; diagnosticar sem métricas; corrigir várias coisas de uma vez.
