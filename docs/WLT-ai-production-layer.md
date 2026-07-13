# AI Production Prompt Layer — LT-squad-pro

Camada que transforma **artefato estratégico aprovado** em **prompt executável** por outra IA.
Dono: `WLT-ai-production-prompt-engineer`. Ele **não decide estratégia** — só traduz.

## Menu (após cada artefato aprovado)
```
[1] Continuar para a próxima etapa
[2] Gerar prompt para Codex / Claude Code
[3] Gerar prompt para Lovable / Antigravity
[4] Gerar prompt para outra LLM
[5] Gerar versão para revisão humana
```

## Regra de bloqueio (obrigatória)
Não gera prompt de produção se o artefato falhou no checklist:
- Sem oferta aprovada → não gera landing page.
- Sem product brief aprovado → não gera ebook.
- Sem blueprint de agente aprovado → não gera agente de IA.
- Sem página + criativos → não gera tráfego.
- Sem caso de uso + inputs/outputs claros → não gera micro app.

## Roteamento por artefato
| Artefato | Task | Template | Skill externa |
|---|---|---|---|
| ebook | WLT-generate-ebook-generation-prompt | WLT-ebook-generation-prompt-template | — |
| landing page | WLT-generate-landing-page-prompt | WLT-landing-page-prompt-template | frontend-design |
| agente de IA | WLT-generate-ai-agent-prompt | WLT-ai-agent-prompt-template | assistant-creator-v2 |
| micro app | WLT-generate-micro-app-prompt | WLT-micro-app-prompt-template | — |
| design system | WLT-generate-design-system-prompt | WLT-claude-code-prompt-template | frontend-design |
| scripts/automação | WLT-generate-codex-prompt | WLT-codex-prompt-template | — |
| componentes/frontend | WLT-generate-claude-code-prompt | WLT-claude-code-prompt-template | frontend-design |
| genérico | WLT-generate-implementation-prompt | WLT-implementation-prompt-template | — |
| criativos retargeting/bump/upsell | WLT-generate-static-ads + WLT-generate-implementation-prompt (role_in_funnel na matriz) | WLT-static-ads-template | — |

## Plataformas alvo
Codex · Claude Code · Claude Design · Antigravity · Lovable · Cursor · Replit · v0 · Bolt · outra LLM (ver `data/WLT-ai-production-targets.yaml`).

## Regras específicas
- **Landing page:** GSAP (não GASP), preço só na dobra 9, CTA verbal, sem depoimento inventado, mobile-safe.
- **Agente de IA:** resolve tarefa específica; system prompt completo; anti-alucinação; testes simulados.
- **Ebook:** objetivo, prático, consumível; saída em Markdown; nada de livro inchado.
- **Todos:** nunca contradizer Offer Doc / Product Brief / Avatar Dossier.

## Fluxo real de referência (transcrição Pedro Aredes)
persona → soluções → produto → blueprint → prompt (Manus/Claude gera o produto) → página 12 dobras →
prompt (Lovable monta a landing) → criativos → análise de tráfego/página/criativo. A LT-squad-pro reproduz
isso com bloqueio de qualidade em cada passo.
