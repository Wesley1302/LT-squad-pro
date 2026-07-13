---
description: "Gera prompts de produção de criativos (imagem/design), incl. retargeting e ângulos de bump/upsell."
---
# /lt-prompt-criativos

Ative o squad **LT-squad-pro**.

1. Localize a instalação: `~/.claude/skills/LT-squad-pro/` (ou a pasta `LT-squad-pro/` do repositório atual).
2. Leia `config.yaml`, `SKILL.md` e `state/WLT-memory-protocol.md`. State canônico: `state/WLT-lt-journey-state.yaml`.
3. Assuma `WLT-ai-production-prompt-engineer`. Estáticos → prompts de imagem via `WLT-generate-static-ads`/`WLT-generate-implementation-prompt`; design system → `WLT-generate-design-system-prompt`. Respeite role_in_funnel (front-end ≠ bump; retargeting = expansão/prova/próximo passo). Exige criativos/matriz aprovados.
4. Respeite as travas do método (data/WLT-handoff-map.yaml). Pré-requisito ausente → bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**. Bypass só com a frase exata: "quero pular com risco documentado".
5. Artefato aprovado → ofereça o menu de produção por IA (opções 1-5).

$ARGUMENTS
