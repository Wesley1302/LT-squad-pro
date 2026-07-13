# Tracking Readiness Checklist (BLOQUEANTE)

- [ ] Pixel criado e instalado na página de vendas (código base no head)
- [ ] Pixel instalado/configurado na plataforma de checkout (ID/token validados)
- [ ] CAPI (API de Conversões) ativa — setup conforme doc oficial (documentacao-oficial-verificar)
- [ ] Evento de compra (Purchase) disparando — teste real ou evento de teste no Gerenciador de Eventos
- [ ] Meta Pixel Helper verde na landing publicada
- [ ] Colunas de análise configuradas (padrão + personalizadas: caixa, taxa compra página, taxa checkout, connect rate, CPV)
- [ ] UTMs/nomenclatura definidas (produto+tipo+criativo; utm_content identifica o criativo)
- [ ] Deduplicação conferida quando Pixel+CAPI enviam o mesmo evento (event_id — documentacao-oficial-verificar)

**Decisão:** PASS → WLT-build-traffic-test-plan · FAIL → completar item ausente · BYPASS → "quero pular com risco documentado"
