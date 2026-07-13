# Mapa de Workflows — LT-squad-pro

| Workflow | Quando | Entrada | Fim |
|---|---|---|---|
| `WLT-lt-from-zero` | Não sei o que vender | WLT-lt-chief | WLT-build-next-offer-plan |
| `WLT-lt-with-existing-niche` | Tenho nicho, falta produto | WLT-lt-chief | WLT-build-traffic-test-plan |
| `WLT-lt-with-existing-product` | Já tenho produto | WLT-lt-chief (audita) | WLT-build-traffic-test-plan |
| `WLT-product-validation-cycle` | Validar produto | WLT-product-miojo-builder | — |
| `WLT-offer-build-cycle` | Construir oferta | WLT-offer-architect | WLT-validate-offer |
| `WLT-deliverable-production-cycle` | Produzir entregáveis | WLT-deliverable-builder | ai-production |
| `WLT-landing-page-production-cycle` | Página / prompt de landing | WLT-sales-page-builder | WLT-generate-landing-page-prompt |
| `WLT-creative-testing-cycle` | Criativos + teste | WLT-creative-intelligence-analyst | WLT-build-traffic-test-plan |
| `WLT-funnel-diagnosis-cycle` | Gastei e não vendi | WLT-funnel-diagnostician | route-to-owner |
| `WLT-scale-cycle` | Vendo e quero escalar | WLT-scale-architect | WLT-build-next-offer-plan |
| `WLT-ai-production-prompt-cycle` | Prompt pra outra IA | WLT-ai-production-prompt-engineer | done |

## Fluxo principal (WLT-lt-from-zero)
```
diagnóstico → money-skill → nicho → avatar → vozes → demanda → sapato →
produto → [valida] → order bumps → [valida] → oferta(promessa→bigidea→mecanismo→stack)[valida] →
entregáveis → página[valida] → inteligência criativa → vídeo → estático →
direção de gravação → setup de rastreamento → tráfego teste → diagnóstico → (pré-escala) → escala → próxima oferta
```
Após cada artefato aprovado: menu [1] continuar [2] Codex/Claude Code [3] Lovable/Antigravity [4] outra LLM [5] revisão humana.

## Branch de produção por IA
Qualquer etapa aprovada → (opção 2/3/4) → `WLT-ai-production-prompt-cycle` → gate-check → task de prompt por artefato.
Gate bloqueia se o artefato falhou no checklist.
