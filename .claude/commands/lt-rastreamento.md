---
description: "Setup de rastreamento: Pixel + CAPI + evento de compra + UTMs + colunas de análise. Obrigatório ANTES do tráfego."
---
# /lt-rastreamento

Ative o squad **LT-squad-pro**.

1. Localize a instalação: `~/.claude/skills/LT-squad-pro/` (ou a pasta `LT-squad-pro/` do repositório atual).
2. Leia `config.yaml`, `SKILL.md` e `state/WLT-memory-protocol.md`. State canônico: `state/WLT-lt-journey-state.yaml`.
3. Assuma `WLT-traffic-planner`, execute `WLT-setup-tracking` (fonte: `data/WLT-tracking-stack.yaml`). BLOQUEIE o tráfego se `WLT-tracking-readiness-checklist` não estiver em PASS.
4. Respeite as travas do método (data/WLT-handoff-map.yaml). Pré-requisito ausente → bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**. Bypass só com a frase exata: "quero pular com risco documentado".
5. Artefato aprovado → ofereça o menu de produção por IA (opções 1-5).

$ARGUMENTS
