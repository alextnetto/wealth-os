---
name: rotina-financeira
description: >-
  Executa a rotina financeira mensal do wealth-os: coleta de extratos novos, classificação,
  consolidação, republicação de planilha/agenda e retrato do mês. Use quando o usuário disser
  "rotina do mês", "fecha o mês", "roda a rotina financeira", "rotina do dia <N>", "fechamento
  mensal" — ou quando chegar o dia da rotina marcado na agenda "Finanças". Só se aplica a repo
  com onboarding concluído (existe `plan/rotina-mensal.md`).
---

# Rotina financeira mensal — executar o checklist e entregar o retrato

Esta skill só dispara e aponta. **O checklist personalizado do titular mora em
`plan/rotina-mensal.md`** — é ele que se executa, passo a passo, não uma versão de memória.

## Pré-checagens (antes de qualquer passo)

1. `plan/rotina-mensal.md` existe? Se não, o onboarding não terminou — use a skill
   `onboarding-patrimonial`, que detecta a fase e retoma.
2. Privacidade: se a sessão for fazer push, `git remote -v` primeiro (checklist em
   `principios/privacidade.md`).
3. Anuncie qual mês está sendo fechado e o que a rotina vai tocar.

## Execução

- Siga `plan/rotina-mensal.md` na ordem: coletar extratos/faturas novos → classificar → baixas
  nos ledgers → atualizar consolidados → republicar planilha/agenda → revisar pendências e
  vencidos.
- **Leia o stdout inteiro dos scripts, não só o exit code** — guards imprimem `AVISO` + pendência
  estruturada, e "rodou sem erro" ≠ "nada a investigar" (doutrina em
  `principios/arquitetura.md`).
- O que depender do titular vira pendência com valor em jogo em `plan/proximos-passos.md`
  (formato em `principios/governanca.md`) — nunca nota solta no chat.
- Nunca execute transação financeira: a rotina prepara, agenda e lembra; quem paga é o titular.

## Encerramento — o retrato do mês

Termine sempre com o retrato para o titular: sobra do mês (entradas − fixos − variável), desvios
de orçamento por categoria, alertas dos guards, vencimentos dos próximos 30 dias e pendências
abertas (novas e vencidas). Curto, com números, e com o que precisa de resposta dele em destaque.
