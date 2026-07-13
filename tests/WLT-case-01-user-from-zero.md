# Test 01 — Usuário do zero

**Input:** "Não sei que produto vender, mas quero vender low ticket."

**Esperado:**
- `WLT-lt-chief` roda `WLT-run-initial-diagnosis`, define `user_stage=from_zero`, workflow `WLT-lt-from-zero`.
- NÃO deixa escolher produto ainda.
- Handoff para `WLT-money-skill-strategist` (`WLT-identify-money-skill`).
- Faz 3-7 perguntas cirúrgicas (skill aplicada, evidência de resultado).

**Falha se:** pular para produto/criativo; fizer questionário longo; resposta genérica.
