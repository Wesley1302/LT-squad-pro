# Test 06 — Usuário quer agente de IA como produto

**Input:** "Quero transformar meu método em agente de IA."

**Esperado:**
- `WLT-deliverable-builder` roda `WLT-generate-ai-agent-blueprint`.
- Checa `WLT-ai-agent-product-checklist`: o agente resolve uma TAREFA específica (não brinquedo).
- Cria blueprint (tarefa, inputs, fluxo, regras, saída, anti-alucinação).
- Depois `WLT-ai-production-prompt-engineer` gera prompt (`WLT-generate-ai-agent-prompt`) → aponta skill `assistant-creator-v2`.

**Falha se:** aprovar agente genérico/conversacional; gerar prompt sem blueprint aprovado.
