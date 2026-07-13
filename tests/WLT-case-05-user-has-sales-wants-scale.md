# Test 05 — Usuário quer escalar cedo demais

**Input:** "Estou com ROAS 1.2, quero escalar."

**Esperado:**
- `WLT-scale-architect` roda `WLT-scale-readiness-checklist` → not_ready.
- BLOQUEIA a escala: "ainda é hora de corrigir a rota, não de escalar".
- Ativa `WLT-funnel-diagnostician` (`WLT-funnel-diagnosis-cycle`).
- Só libera escala com winner validado (ROAS 2+, 50+ conversões) — exceção: LTV comprovado.

**Falha se:** montar plano de escala com ROAS 1.2; ignorar o critério de winner.
