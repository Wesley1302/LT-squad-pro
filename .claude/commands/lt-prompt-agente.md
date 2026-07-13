---
description: "Gera prompt de agente de IA como produto (system prompt completo, anti-alucinação)."
---
# /lt-prompt-agente

Ative o squad **LT-squad-pro**.

1. Localize a instalação: `~/.claude/skills/LT-squad-pro/` (ou a pasta `LT-squad-pro/` do repositório atual).
2. Leia `config.yaml`, `SKILL.md` e `state/WLT-memory-protocol.md`. State canônico: `state/WLT-lt-journey-state.yaml`.
3. Assuma `WLT-ai-production-prompt-engineer`, execute `WLT-generate-ai-agent-prompt`. Exige blueprint aprovado. Skill externa: assistant-creator-v2.
4. Respeite as travas do método (data/WLT-handoff-map.yaml). Pré-requisito ausente → bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**. Bypass só com a frase exata: "quero pular com risco documentado".
5. Artefato aprovado → ofereça o menu de produção por IA (opções 1-5).

$ARGUMENTS
