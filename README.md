# LT-squad-pro

Squad modular de agentes para **construir, validar e escalar produtos low ticket** — do
"não sei o que vender" até "validei, escalei e preciso decidir a próxima oferta/upsell/esteira".

Baseado no método low ticket (Pedro Aredes / ProAlt / Desafio Low Ticket). Arquitetura no padrão
de referência AIOX-core: **Agent → Task → Workflow → Handoff → State** (+ Artifacts, Checklists e AI Production Prompt Layer).

> Filosofia: **Produto certo. Oferta óbvia. Criativo forte. Página que converte. Tráfego só depois.**
> Este squad não é professor — é uma máquina de decisão e execução.

## Para quem é
- Quem quer criar um produto low ticket do zero.
- Quem tem nicho/produto e quer estruturar oferta, página, criativos e tráfego.
- Quem rodou tráfego, travou e precisa diagnosticar o gargalo.
- Quem vende e quer escalar / decidir a próxima oferta.

## Para quem NÃO é
- Quem quer high ticket, lançamento ou funil complexo.
- Quem quer "ideias soltas" — aqui tudo vira artefato e decisão.
- Quem quer pular etapas sem assumir risco (o squad bloqueia).

## Arquitetura
```
LT-squad-pro/
├── config.yaml            # manifesto do squad (tiers, agents, handoffs, settings, activation)
├── tool-overrides.yaml    # ferramentas por agent + fallback manual
├── SKILL.md               # projeção do WLT-lt-chief (entrada p/ Claude Code)
├── .claude/commands/ (24) # camada visível: /lt-chief, /lt-produto, /lt-rastreamento...
├── agents/    (17)        # quem decide/executa (WLT-lt-chief + Tier 1 + Tier 2)
├── tasks/     (40)        # unidades executáveis (inputs->steps->outputs->handoff)
├── workflows/ (11)        # orquestração (next/on_failure)
├── checklists/(24)        # validadores bloqueantes (PASS/FAIL/BYPASS)
├── templates/ (25)        # contratos de artefato + templates de prompt de produção
├── data/      (16)        # o método low ticket codificado (fonte de verdade)
├── state/     (3)         # jornada + sessão + WLT-memory-protocol
├── tests/     (13)         # casos simulados
└── docs/      (6)         # arquitetura, contratos, camada de IA, uso
```

## Ordem inquebrável
Diagnóstico → Money Skill → Nicho → Avatar → Demanda → Produto Miojo → Validação do Produto → Order Bumps → Validação dos Order Bumps → Oferta Completa → Entregáveis →
Página → Inteligência Criativa → Criativos → Direção de Gravação → Rastreamento (Pixel+CAPI) → Tráfego → Diagnóstico de Gargalo → Pré-escala → Escala → Esteira.

**Order bumps são etapa estrutural** (não detalhe): pós-validação do Miojo, pré-oferta, ~30% do lucro. Gate: `WLT-order-bump-checklist`.

## Como instalar
Copie `LT-squad-pro/` para o seu runtime de squads (padrão AIOX-core / Claude Code). Entry: `WLT-lt-chief`.

## Comandos (camada lt-*)
Entrada única: **`/lt-chief`** — diagnostica seu estágio e conduz até o fim.
`lt-diagnostico · lt-money-skill · lt-nicho · lt-avatar · lt-demanda · lt-produto · lt-order-bumps · lt-oferta · lt-entregaveis · lt-pagina · lt-inteligencia-criativa · lt-criativos · lt-direcao · lt-rastreamento · lt-trafego · lt-gargalo · lt-escala · lt-esteira · lt-prompt-landing · lt-prompt-agente · lt-prompt-ebook · lt-prompt-micro-app · lt-prompt-criativos`
(`.claude/commands/` · mapa em `config.yaml -> commands` · interno = `WLT-*`)

## Como ativar
Carregue `config.yaml`. O `WLT-lt-chief` inicia o diagnóstico e escolhe o workflow. Ver `docs/WLT-usage-guide.md`.

## Como rodar cada workflow
`docs/WLT-workflow-map.md` lista os 11 workflows e quando usar cada um. Ex.:
- Do zero → `WLT-lt-from-zero`
- Gastei e não vendi → `WLT-funnel-diagnosis-cycle`
- Quero prompt de landing → `WLT-landing-page-production-cycle`

## Como funcionam Agents / Tasks / State / Prompts
- **Agents** (`agents/*.md`): persona, commands, dependencies, tools, restrictions, handoff, memory.
- **Tasks** (`tasks/*.md`): unidade executável reutilizável (a "skill" é só uma projeção de Agent).
- **State** (`state/*`): fonte de verdade entre etapas; ver `state/WLT-memory-protocol.md`.
- **AI Production Prompt Layer**: após cada artefato aprovado, gere prompt pra Codex/Claude Code/Lovable/etc. Ver `docs/WLT-ai-production-layer.md`.

## Gerar SKILL.md para Claude Code
O `SKILL.md` é uma **projeção** de um Agent (a lógica canônica vive na Task). Passo a passo em `docs/WLT-usage-guide.md → item 8`.

## Skills externas integradas
- `assistant-creator-v2` — construir agentes de IA (quando o produto é agente/GPT).
- `frontend-design` — UI/UX, landing, micro app, design system.
- `ui-ux-pro-max` — UX/UI e design system da landing (antes de gerar o prompt).
- `impeccable` — QA e polimento de frontend (após implementar).
- `carrossel-creator-v3` — referência de hooks (inspiração estrutural).

## Regras que o squad nunca quebra
- Nunca inventa prova, pesquisa ou depoimento (Evidence Ledger; plano ético de prova).
- Nunca deixa avançar sem pré-requisito (bloqueio: risco → pré-req → redireciona; bypass só com frase exata).
- Nunca gera prompt de produção de artefato reprovado no checklist.
- Diferencia sempre: `existe-no-metodo` | `convencao-observada` | `extensao-lt-squad-pro`.

## Troubleshooting
Ver `docs/WLT-usage-guide.md → item 10`.
