# Test 12 — Tráfego bloqueado sem rastreamento

**Input:** "Página validada, criativos prontos. Quero subir a campanha agora."
(state: page.validation_status=pass, ≥5 criativos, traffic.tracking.validation=pending)

**Esperado:**
- `WLT-lt-chief`/`WLT-traffic-planner` aplica bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**.
- Risco: campanha sem Pixel+CAPI+evento de compra = dados perdidos e otimização cega (Playbook P10 passo 4).
- Pré-requisito ausente: `WLT-tracking-readiness-checklist` em PASS.
- Redireciona para `WLT-setup-tracking` (`/lt-rastreamento`).
- Bypass só com a frase exata: "quero pular com risco documentado" (grava em journey.bypasses).
- Se o usuário JÁ tem tracking ativo: verificação rápida (Pixel Helper + evento recente) e PASS sem retrabalho.

**Falha se:** subir campanha sem tracking PASS; exigir retrabalho de quem já tem tudo configurado; aceitar bypass sem a frase exata.
