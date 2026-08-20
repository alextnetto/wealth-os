<!-- instruções para o agente:
Copie para `plan/rotina-mensal.md` na F6 e personalize: uma linha de coleta por fonte real
declarada na F2, o dia escolhido na F0, e o método de publicação registrado em
plan/ferramentas.md (gog, MCP ou fallback local). Este arquivo é o roteiro que a skill
rotina-financeira executa todo mês. Mantenha-o executável: cada passo com ação ou comando
concreto, não intenção vaga. O agente conduz a primeira rotina do início ao fim. Ao
personalizar, apague este comentário e os placeholders não usados.
-->

# Rotina financeira mensal: dia <D>

Duração-alvo: 30 a 45 minutos. Fecha o mês anterior. A ordem dos passos é contrato: classificar
antes de consolidar, consolidar antes de publicar. O espelho só é republicado depois de a fonte
da verdade estar atualizada.

## 1. Coletar dados novos (titular exporta, agente arquiva)

- [ ] <fonte 1; ex.: extrato CSV do mês fechado, app do banco> → `fontes/<fonte>/raw/`
- [ ] <fonte 2; ex.: fatura PDF do cartão> → `fontes/<fonte>/raw/`
- [ ] <fonte N; uma linha por fonte declarada na F2>
- [ ] Nomear pelo período coberto (ex.: `extrato-2026-07.csv`). Raw é imutável: nunca editar.

## 2. Classificar

- [ ] Rodar a classificação dos gastos novos (precedência: exceções > regex > de-para >
      `a-classificar`)
- [ ] Reduzir `a-classificar` do mês fechado a < 10%: gasto recorrente ganha regra em
      `regras-classificacao.csv`; caso pontual ganha linha em `excecoes.csv`

## 3. Baixas e ledgers

- [ ] Dar baixa nas obrigações do mês **no ledger da fonte** (`fontes/<fonte>/ledger.csv`,
      coluna `status`: previsto → pago, registrando o valor real). Nunca em
      `recorrentes.csv`, que guarda só a definição da obrigação, nem no consolidado, que é
      regenerado
- [ ] Atualizar saldos manuais com `data_ref` vencida (`saldos-manuais.csv`). Célula sem
      fonte é célula vazia + pendência

## 4. Consolidar

- [ ] Rodar os consolidadores do repo (fluxo de caixa, patrimônio). Falha interrompe a
      rotina: parar e investigar, nunca prosseguir com erro

## 5. Republicar espelhos

- [ ] Planilha mestra (6 abas). Republicação regenerável: mesmo input, mesmo resultado
- [ ] Agenda de vencimentos. Idempotente via `agenda-map.csv`: atualiza, nunca duplica
- [ ] <sem Google: regerar `plan/consolidado/*.csv` + `dashboard.html`>

## 6. Pendências e vencidos

- [ ] Revisar `proximos-passos.md`: fechar itens concluídos (✅ + data), marcar vencidos 🔴,
      datar novos
- [ ] Revisar vencimentos dos próximos 60 dias (consolidado de obrigações), essencial no
      fallback sem agenda Google
- [ ] Regerar `perguntas-abertas.md` a partir do registro

## 7. Retrato do mês (entregável ao titular)

- [ ] Sobra do mês (entradas − saídas) vs projetado
- [ ] Top 3 desvios de orçamento (orçado × realizado), com hipótese de causa
- [ ] Alertas: vencimentos dos próximos 30 dias, classes fora da banda do IPS, riscos com
      gatilho disparado
- [ ] Perguntas que só o titular responde (da vista consolidada), uma por vez
