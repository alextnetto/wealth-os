---
name: onboarding-patrimonial
description: >-
  Conduz o onboarding do wealth-os: entrevista de mandato, inventário de fontes, orçamento,
  snapshot de patrimônio, planilha/agenda e rotina. Use quando o usuário disser "faz meu
  onboarding", "configura minhas finanças", "monta meu sistema financeiro", "wealth-os", "começa o
  setup", "continua o onboarding" — ou em qualquer primeiro contato com um repo wealth-os sem
  `plan/` ou com onboarding incompleto (fases F0–F6 pela metade). Detecta a fase pelo estado do
  repo e retoma de onde parou.
---

# Onboarding patrimonial — detectar a fase e seguir o protocolo

Esta skill só detecta e aponta. **O protocolo mora em `onboarding/`** — abra e siga os arquivos de
lá; não conduza de memória.

## Procedimento

1. **Detecte a fase** pelo estado do repo (a tabela completa e autoritativa está em `AGENTS.md`):

   | Estado do repo | Fase | Protocolo |
   |---|---|---|
   | não existe `plan/` OU falta `plan/perfil.md` ou `plan/ferramentas.md` | F0 | `onboarding/f0-preparacao.md` |
   | `plan/` sem `plan/mandato.md` | F1 | `onboarding/f1-mandato.md` |
   | mandato sem `fontes/` populado | F2 | `onboarding/f2-fontes-e-extratos.md` |
   | orçamento incompleto: falta `plan/orcamento.csv`, `plan/recorrentes.csv` ou `plan/categorias.csv` | F3 | `onboarding/f3-orcamento-e-recorrentes.md` |
   | orçamento sem `plan/patrimonio.csv` | F4 | `onboarding/f4-patrimonio.md` |
   | patrimônio sem espelhos publicados | F5 | `onboarding/f5-planilha-e-agenda.md` |
   | espelhos sem `plan/rotina-mensal.md` ou `plan/riscos.md` | F6 | `onboarding/f6-rotina-e-governanca.md` |
   | tudo existe | operação | esta skill não se aplica — use `rotina-financeira` |

2. **Anuncie** ao titular em que fase o repo está e o que falta para fechá-la — o onboarding é
   retomável e leva 2–5 sessões; nunca finja que está começando do zero se não estiver.

3. **Abra `onboarding/README.md`** (visão geral, gates, como conduzir) **e o arquivo da fase
   atual**, e siga o protocolo de lá.

## Regras que valem em toda fase

- **Uma pergunta por vez**; múltipla escolha quando possível; registrar a resposta antes da
  próxima. "Não sei / depois" vira pendência em `plan/proximos-passos.md`, nunca trava a fase.
- **Gates**: só avance de fase quando os artefatos existirem e o titular confirmar.
- Doutrina completa em `principios/` (arquitetura, governança, privacidade) — consulte quando o
  protocolo da fase não bastar. Privacidade primeiro: a F0 executa o checklist de
  `principios/privacidade.md` antes de qualquer dado entrar.
