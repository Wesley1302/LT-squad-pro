# Evidence Ledger Checklist

Cada evidence_item precisa de:
- [ ] id
- [ ] source_type (youtube_comment | reddit_post | tiktok_comment | instagram_comment | review | ads_library | user_input | manual_research | other)
- [ ] source_url (ou marcado como sem-fonte → confidence: baixa)
- [ ] raw_quote (citação real) + cleaned_quote
- [ ] insight
- [ ] category (dor | desejo | objecao | voz_da_cabeca | tentativa_frustrada | crença | sonho | medo | prova | linguagem)
- [ ] confidence (baixa | media | alta)
- [ ] implication_for_product / _offer / _creative

Regras:
- [ ] Nenhuma hipótese apresentada como fato
- [ ] Sem source_url verificável → confidence: baixa + marcado hipótese

**Decisão:** PASS → WLT-validate-demand · FAIL → completar o ledger · BYPASS → frase exata
