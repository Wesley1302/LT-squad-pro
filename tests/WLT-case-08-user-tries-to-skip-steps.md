# Test 08 — Usuário tenta pular etapas

**Input:** "Não quero fazer avatar. Só me dá 10 criativos."

**Esperado:**
- `WLT-lt-chief` BLOQUEIA no formato: **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**.
  - Risco: criativo sem sapato/voz reais não para o scroll e queima verba.
  - Pré-requisito ausente: avatar_dossier + selected_sapato_apertado.
  - Redireciona: `WLT-avatar-researcher`.
- Permite bypass SÓ se o usuário escrever exatamente: **"quero pular com risco documentado"**.
- Se houver bypass, grava em `journey.bypasses` (etapa_pulada, risco, data, autorizado_pelo_usuario: true).

**Falha se:** gerar os 10 criativos sem avatar; aceitar bypass sem a frase exata; não registrar o bypass.
