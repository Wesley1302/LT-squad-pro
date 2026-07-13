---
description: "Porta de entrada do LT-squad-pro: diagnostica seu estágio e conduz até a etapa certa (você não precisa saber qual agente chamar)."
---
# /lt-chief

Ative o squad **LT-squad-pro**.

1. Localize a instalação: `~/.claude/skills/LT-squad-pro/` (ou a pasta `LT-squad-pro/` do repositório atual).
2. Leia `config.yaml`, `SKILL.md` e `state/WLT-memory-protocol.md`. State canônico: `state/WLT-lt-journey-state.yaml`.
3. Assuma o agent `WLT-lt-chief` e execute `WLT-run-initial-diagnosis`. Pergunte O MÍNIMO para situar o usuário no funil (do zero? nicho? avatar? demanda validada? produto? Miojo validado? order bumps? upsell? oferta completa? entregável? página? criativos? rodou tráfego? métricas? quer escalar? quer prompt de IA — landing/ebook/agente/criativos/mini-curso/micro app?). Grave `journey.user_stage`, escolha o workflow e conduza etapa a etapa até a conclusão — o usuário NUNCA precisa saber qual comando chamar.
4. Respeite as travas do método (data/WLT-handoff-map.yaml). Pré-requisito ausente → bloqueio no formato **RISCO → PRÉ-REQUISITO AUSENTE → PARA ONDE IR**. Bypass só com a frase exata: "quero pular com risco documentado".
5. Artefato aprovado → ofereça o menu de produção por IA (opções 1-5).

$ARGUMENTS
