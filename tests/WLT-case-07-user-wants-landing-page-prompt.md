# Test 07 — Usuário quer prompt de landing page

**Input:** "Já tenho produto e oferta. Quero prompt para criar a landing page no Lovable."

**Esperado:**
- Checa `offer.validation_status=pass`. Se oferta não validada → BLOQUEIA (WLT-offer-build-cycle).
- Checa a página nas 12 dobras (write/WLT-validate-sales-page se ainda não existe).
- Se aprovado → `WLT-ai-production-prompt-engineer` (`WLT-generate-landing-page-prompt`, target=lovable).
- Prompt inclui Estratégia + UI/UX + Animações (GSAP/ScrollTrigger/Three.js, mobile-safe) + Engenharia + Restrições.
- Preço só na dobra 9, CTA verbal, sem depoimento inventado. GSAP (não GASP).

**Falha se:** gerar landing com oferta reprovada; escrever "GASP"; preço antes da dobra 9; CTA "Comprar"; inventar depoimento.
