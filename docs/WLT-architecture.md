# Arquitetura — LT-squad-pro

## Padrão
Segue o padrão de referência (AIOX-core): **Agent → Task → Workflow → Handoff → State**, com camadas
adicionais **Artifacts** e **Validators/Checklists** e a **AI Production Prompt Layer**.

A unidade executável reutilizável é a **Task** (não a "skill"). Um `SKILL.md` de Claude Code é uma
*projeção* gerada a partir de um Agent (ver `docs/WLT-usage-guide.md → Gerar SKILL.md`).

```
Squad (config.yaml)
 └─ Agents (agents/*.md)            quem decide/executa
     └─ Tasks (tasks/*.md)          unidade executável (inputs->steps->outputs->handoff)
         └─ Workflows (workflows/*) orquestram tasks em sequência (next/on_failure)
             └─ Handoffs (data/WLT-handoff-map.yaml + docs/WLT-handoff-contracts.md)
                 └─ State (state/*) fonte de verdade entre etapas
                     └─ Artifacts (templates/*) saída persistida de cada task
                         └─ Validators (checklists/*) PASS/FAIL/BYPASS
                             └─ AI Production Prompt Layer (WLT-ai-production-prompt-engineer)
```

## Tiers
- **Tier 0** — `WLT-lt-chief`: diagnóstico, roteamento, bloqueio, QA, state.
- **Tier 1** — Strategy Core: money-skill, niche, avatar, demand, product, offer.
- **Tier 2** — Production & Execution: deliverables, page, creatives, recording, traffic, diagnosis, scale, AI production prompts.

## Ordem inquebrável (método low ticket)
Diagnóstico → Money Skill → Nicho → Avatar → Demanda → Produto Miojo → Validação do Produto → Order Bumps → Validação dos Order Bumps → Oferta Completa → Entregáveis →
Página → Inteligência Criativa → Criativos → Direção de Gravação → Tráfego → Diagnóstico de Gargalo → Escala → Esteira.

Regras de precedência: Tráfego nunca antes de produto+criativo+página · Criativo nunca antes de produto+oferta ·
Produto nunca antes de avatar+demanda · Order bumps nunca antes de produto validado · Oferta nunca antes de bumps validados · Página nunca antes de oferta ·
Prompt de produção nunca antes do artefato estratégico aprovado.

## Decisões arquiteturais
| Decisão | Origem |
|---|---|
| Agent/Task/Workflow/Handoff/State | existe-no-padrão (AIOX-core) |
| Tiers 0/1/2 | convenção observada |
| Evidence Ledger obrigatório | extensão-lt-squad-pro (rigor anti-alucinação) |
| Checklists bloqueantes com PASS/FAIL/BYPASS | extensão-lt-squad-pro |
| AI Production Prompt Layer | extensão-lt-squad-pro (obrigatória no brief) |
| Menu de decisão (1-5) após cada artefato | extensão-lt-squad-pro |
| Conteúdo do método (data/*) | existe-no-método (Pedro Aredes / Playbook V3) |
| Order bumps como etapa obrigatória pós-produto | existe-no-método (Bloco 5: ~30% do lucro) + extensão (gates próprios) |
| Camada de comandos lt-* (.claude/commands) | extensão-lt-squad-pro (UX estilo prefixo de squad) |
| Camada de rastreamento (WLT-tracking-stack + WLT-setup-tracking) | existe-no-metodo (mod.10, Pixel/CAPI/colunas) + extensão (UTMs, eventos, dedup — doc oficial a verificar) |
| Economia do funil (WLT-funnel-economics) | existe-no-metodo (mod.14: bump 30-40%, upsell ~5%, downsell, ticket médio) |
| Pré-escala como estágio explícito | extensão-lt-squad-pro (consolida regras do mod.11 §7 / playbook P05) |
| Landing 100% vibe coding (Claude Code/Codex/Antigravity/Lovable + frontend-design/ui-ux-pro-max/impeccable) | diretriz do usuário (WordPress/Elementor removidos) |
| Mínimo 5 criativos/rodada (ideal 10+) | existe-no-metodo (correção de fidelidade: mod.11 §4) |
| role_in_funnel na matriz criativa | extensão-lt-squad-pro (criativos ↔ oferta completa/retargeting) |

## Nomeação
"AIOX" não aparece no nome, pastas, branding ou identidade — usado apenas como referência técnica.
