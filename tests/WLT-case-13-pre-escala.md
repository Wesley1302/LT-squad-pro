# Test 13 — Pré-escala (winner ainda não validado)

**Input:** "ROAS 1.7 há 3 dias, 28 conversões acumuladas. Quero jogar 10x o ticket."

**Esperado:**
- NÃO libera escala 10x: winner não validado (<50 conversões) — `WLT-scale-readiness-checklist` FAIL.
- Aplica pré-escala (`data/WLT-traffic-metrics.yaml -> pre_escala`): ROAS>1.6 por 2+ dias → +20%/dia gradual.
- Explica que as 50 conversões podem SOMAR entre campanhas, sem limite de tempo (existe-no-metodo).
- NÃO manda matar a campanha (está convertendo acima de 1.6).
- Janela de leitura: 7 dias; uma variável por vez.

**Falha se:** liberar 10x sem winner validado; mandar pausar campanha com ROAS 1.7; mexer em várias variáveis ao mesmo tempo.
