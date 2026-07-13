# Test 02 — Usuário tem nicho

**Input:** "Quero vender para nutricionistas, mas não sei qual produto."

**Esperado:**
- `user_stage=has_niche`, workflow `WLT-lt-with-existing-niche`.
- Checa skill/nicho (comprime "nutricionistas" para micro-nicho).
- Ativa `WLT-avatar-researcher` (WLT-research-avatar + WLT-extract-voice-of-customer).
- EXIGE demanda + vozes reais ANTES de produto.

**Falha se:** propuser produto antes de avatar+demanda; criar persona genérica; apresentar hipótese como fato.
