<!-- instruções pro agente:
Copie para `plan/rotina-mensal.md` na F6 e PERSONALIZE: uma linha de coleta por fonte real
declarada na F2, o dia escolhido na F0, e o método de publicação registrado em
plan/ferramentas.md (gog, MCP ou fallback local). Este arquivo é o roteiro que a skill
rotina-financeira executa todo mês — mantenha-o executável: cada passo com ação ou comando
concreto, não intenção vaga. A primeira rotina é guiada pelo agente de ponta a ponta. Ao
personalizar, apague este comentário e os placeholders não usados.
-->

# Rotina financeira mensal — dia <D>

Duração-alvo: 30–45 min. Fecha o mês anterior. A ordem dos passos é contrato: classificar
antes de consolidar, consolidar antes de publicar — o espelho nunca anda à frente da fonte da
verdade.

## 1. Coletar dados novos (titular exporta, agente arquiva)

- [ ] <fonte 1 — ex.: extrato CSV do mês fechado, app do banco> → `fontes/<fonte>/raw/`
- [ ] <fonte 2 — ex.: fatura PDF do cartão> → `fontes/<fonte>/raw/`
- [ ] <fonte N — uma linha por fonte declarada na F2>
- [ ] Nomear pelo período coberto (ex.: `extrato-2026-07.csv`). Raw é imutável: nunca editar.

## 2. Classificar

- [ ] Rodar a classificação dos gastos novos (precedência: exceções > regex > de-para >
      `a-classificar`)
- [ ] Reduzir `a-classificar` do mês fechado a < 10%: gasto recorrente ganha regra em
      `regras-classificacao.csv`; caso pontual ganha linha em `excecoes.csv`

## 3. Baixas e ledgers

- [ ] Dar baixa nas obrigações do mês NO LEDGER DA FONTE (`fontes/<fonte>/ledger.csv`,
      coluna `status`: previsto → pago, registrando o valor real) — nunca em
      `recorrentes.csv`, que guarda só a definição da obrigação, nem no consolidado, que é
      regenerado
- [ ] Atualizar saldos manuais com `data_ref` vencida (`saldos-manuais.csv`) — célula sem
      fonte é célula vazia + pendência

## 4. Consolidar

- [ ] Rodar os consolidadores do repo (fluxo de caixa, patrimônio). Falha ruidosa = parar e
      investigar, nunca "seguir mesmo assim"

## 5. Republicar espelhos

- [ ] Planilha mestra (6 abas) — republicação regenerável: mesmo input, mesmo resultado
- [ ] Agenda de vencimentos — idempotente via `agenda-map.csv`: atualiza, nunca duplica
- [ ] <sem Google: regerar `plan/consolidado/*.csv` + `dashboard.html`>

## 6. Pendências e vencidos

- [ ] Revisar `proximos-passos.md`: fechar o feito (✅ + data), marcar vencidos 🔴, datar novos
- [ ] Revisar vencimentos dos próximos 60 dias (consolidado de obrigações) — essencial no
      fallback sem agenda Google
- [ ] Regerar `perguntas-abertas.md` a partir do registro

## 7. Retrato do mês (entregável ao titular)

- [ ] Sobra do mês (entradas − saídas) vs projetado
- [ ] Top 3 desvios de orçamento (orçado × realizado), com hipótese de causa
- [ ] Alertas: vencimentos dos próximos 30 dias, classes fora da banda do IPS, riscos com
      gatilho disparado
- [ ] Perguntas que só o titular responde (da vista consolidada) — uma por vez
