# F6: Rotina e governança

**Entrada:** F5 fechada (espelhos publicados). **Saída:** `plan/rotina-mensal.md` personalizado,
`plan/riscos.md` semeado com achados reais, 1º comitê agendado com fila de análises, revisão
anual agendada. **Gate:** checklist de aceite do onboarding aprovado pelo titular.

## Objetivo

As fases F0-F5 construíram o sistema; esta fase o coloca em operação. O onboarding só termina
quando existe uma rotina com dia marcado, um fórum onde decisões acontecem (comitê) e um registro
de riscos que alguém revisa. Sem essas peças, o repo vira um registro sem uso, o mesmo problema
que o titular tinha antes.

O que esta fase instala:

| Peça | Arquivo | Cadência |
|---|---|---|
| Rotina mensal (30 a 45 min) | `plan/rotina-mensal.md` | mensal, dia de `plan/perfil.md` |
| Registro de riscos | `plan/riscos.md` | revisado na rotina; pauta fixa do comitê |
| Comitê trimestral + ata | `plan/decisions.md` | trimestral; 1º em 4 a 6 semanas |
| Fila de análises | itens numerados em `plan/proximos-passos.md` | entregas até o comitê |
| Revisão do mandato | evento anual | anual |

## Rotina mensal

Copie [templates/rotina-mensal.md](../templates/rotina-mensal.md) para `plan/rotina-mensal.md` e
**personalize com as fontes reais do titular**: cada fonte de `fontes/` vira um passo concreto de
coleta ("exportar extrato CSV do app Inter", "baixar fatura PDF do cartão Nubank"), na ordem em
que o titular vai executar. O titular não cumpre uma rotina genérica; cumpre a rotina que
nomeia as fontes reais dele.

Estrutura dos passos (a sequência é fixa; o conteúdo é do titular):

1. **Coletar** extratos/faturas novos de cada fonte → `fontes/<fonte>/raw/`, nomeados pelo
   período. Nunca edite o raw.
2. **Classificar** os gastos novos (precedência da F3: exceções > regex > de-para > fallback
   `a-classificar`; gate: < 10% a-classificar no mês fechado).
3. **Dar baixa** nos ledgers das fontes (`fontes/<fonte>/ledger.csv`, coluna `status`:
   previsto → pago; schema na F3, passo 5). Pago é `status` na fonte, nunca no consolidado.
4. **Atualizar consolidados** (reconstruir do zero) + snapshot novo em `plan/patrimonio.csv`
   (append com a data do dia; nunca edite snapshot antigo).
5. **Republicar** planilha e agenda (upsert idempotente da F5).
6. **Revisar pendências**: itens vencidos de `plan/proximos-passos.md`, vencimentos dos próximos
   60 dias, perguntas abertas que o titular já consegue responder.
7. **Retrato do mês** para o titular: sobra real × projetada, top desvios de orçamento, alertas,
   e o que exige decisão (vai para a pauta do comitê, não para decisão imediata).

Duas regras de condução, com o racional:

- **30 a 45 minutos é teto, não meta.** Se a rotina passa disso, o sistema tem defeito (fonte
  difícil de coletar, classificação incompleta). Conserte o sistema; não alongue a rotina.
- **Rotina não inclui análise.** Achado relevante ("essa dívida está cara") vira pendência ou
  pauta de comitê. Misturar operação com decisão prejudica as duas.

**Primeira rotina: guiada pelo agente do início ao fim.** Agende com o titular a primeira
ocorrência do dia escolhido e conduza passo a passo. Nessa execução o checklist personalizado é
testado e ajustado. Atualize a descrição do evento-âncora (F5) com o checklist final.

## Riscos: semear com achados reais

Copie [templates/riscos.md](../templates/riscos.md) para `plan/riscos.md` e semeie com o que o
onboarding revelou. Risco genérico sem evidência é ruído e leva o titular a ignorar o arquivo.
Onde procurar:

| Achado no onboarding | Risco típico a semear |
|---|---|
| uma classe > 50% dos ativos (F4) | concentração de classe |
| dívida com custo acima do retorno esperado dos ativos (F2/F4) | dívida cara, com pauta de amortização |
| > 80% da renda numa única fonte (F3) | renda concentrada |
| financiamento com coobrigado e sem seguro de vida/invalidez (F2) | lacuna de seguro |
| cripto autocustodiada sem plano de acesso/sucessão (F2) | acesso e sucessão de cripto |
| reserva abaixo do mínimo do mandato (F1 × F3 × F4) | liquidez insuficiente |

Formato de cada risco (contrato do template): **risco, severidade, gatilho de monitoramento,
mitigação, status, revisão**. O gatilho deve ser mensurável com o que o sistema já produz. Risco
que nada monitora não cumpre função. Exemplo (fictício, no formato de tabela do template):

| Risco | Severidade | Gatilho de monitoramento | Mitigação | Status | Última revisão |
|---|---|---|---|---|---|
| concentração imobiliária | alta | imóveis > 50% dos ativos no snapshot mensal | direcionar aportes novos a bolsa/RF até voltar à banda do IPS | aberto | <AAAA-MM-DD> |

Depois de semear, **re-publique a planilha** (F5): a aba Riscos foi publicada vazia e agora tem
conteúdo.

## Comitê trimestral

Decisão patrimonial exige fórum. Fora dele, a decisão vira improviso durante a rotina ou conversa
sem registro. O comitê é o único lugar onde decisão acontece: a rotina opera, o comitê decide, o
mandato limita.

**Pauta fixa, 5 itens, sempre nesta ordem:**

1. **Performance vs política**: patrimônio e resultado do trimestre contra o IPS do mandato.
2. **Desvios e rebalanceamento**: toda classe fora da banda entra aqui com proposta do agente;
   o titular decide (nunca execução automática, doutrina 5).
3. **Riscos**: revisão de `plan/riscos.md`: status, gatilhos disparados, riscos novos.
4. **Análises do trimestre**: entregas da fila (abaixo) com recomendação e opções.
5. **Decisões**: cada uma registrada em `plan/decisions.md` no formato do template (decisão,
   racional, dados no momento, pendências); a ata do comitê vive no mesmo arquivo.

**Agende o 1º comitê para 4 a 6 semanas após o onboarding** (evento na agenda "Finanças"; sem
Google, pendência datada). Racional da janela: perto o bastante para corrigir a calibragem do
sistema enquanto o contexto está recente; longe o bastante para já ter uma rotina mensal
executada e dados reais disponíveis. A pauta do 1º comitê já está definida: os desvios do IPS
registrados na F4 + os riscos semeados acima + as análises abaixo.

## Fila inicial de análises

Monte com o titular a fila personalizada a partir dos achados. Cada item vira linha numerada em
`plan/proximos-passos.md` com data-limite até o 1º comitê. Exemplos (escolha só o que o
onboarding justifica):

- **Amortizar × investir**: se há dívida cara, custo efetivo da dívida vs alternativas líquidas.
- **Estrutura de retirada PJ→PF**: se há PJ, pró-labore × distribuição e carga total legal;
  valide com o contador antes de implementar.
- **Holding imobiliária**: se há imóveis, pessoa física × holding para aluguel e sucessão.
- **Offshore**: se há patrimônio ou renda no exterior, estrutura adequada e obrigações de
  declaração.
- **Previdência**: PGBL × VGBL conforme regime de declaração do IRPF e horizonte.
- **Mapa tributário do patrimônio**: carga atual por fonte de renda e oportunidades legais.
- **Onde alocar a reserva de emergência**: se a reserva existe mas rende mal.

Formato de entrega: análise datada em `docs/relatorios/`, com fontes citadas, premissas
explícitas, cenários e recomendação + opções. Análise sem fonte e sem premissa não vai ao
comitê. Recomendação que o titular não consegue auditar não sustenta decisão. Toda análise
segue [principios/analises.md](../principios/analises.md) e usa o molde
[templates/analise.md](../templates/analise.md); matéria tributária ou jurídica exige validação
de contador ou advogado antes de implementar.

## Revisão anual do mandato

Agende (evento anual na agenda "Finanças"). Revisa-se: o número (bloco 1), marcos de vida, tetos
e vetos, o IPS e as cadências. Racional: o mandato é contrato de longo prazo. Muda quando a vida
muda, mas só nesse rito, com registro em `plan/decisions.md`. Fora do rito, vale o mandato
vigente.

## Encerramento do onboarding

Percorra o checklist de aceite com o titular (espelho dos critérios do
[protocolo](README.md)):

- [ ] Repo privado confirmado; `.env` fora do git (F0)
- [ ] `plan/mandato.md` v1 completo, sem marca "provisório" (F1/F4)
- [ ] Toda fonte declarada tem `fontes/<fonte>/` + README + (raw ou pendência datada) (F2)
- [ ] `plan/orcamento.csv`, `plan/recorrentes.csv`, `plan/categorias.csv` preenchidos;
      a-classificar < 10% no último mês fechado, se houve extrato (F3)
- [ ] `plan/patrimonio.csv` com snapshot completo, passivos negativos; PL vs IPS apresentado (F4)
- [ ] Planilha 6 abas regenerável (ou fallback) + agenda sem duplicatas na re-publicação (F5)
- [ ] `plan/rotina-mensal.md` personalizado; primeira rotina executada guiada, ou agendada (F6)
- [ ] `plan/riscos.md` semeado com achados reais; aba Riscos re-publicada (F6)
- [ ] 1º comitê agendado (4 a 6 semanas) com pauta e fila de análises numeradas (F6)
- [ ] Revisão anual do mandato agendada (F6)
- [ ] Pendências restantes listadas em `plan/proximos-passos.md`, cada uma com data-limite

Liste o que fica pendente. Encerrar com pendência declarada é correto; encerrar como completo
com pendências ocultas não é. Commit: `f6: rotina e governança instaladas`.

Feche com a frase de rito: **"Onboarding encerrado. O sistema agora opera em rotina. Nos vemos
na rotina do dia <dia>."** A partir daqui o agente atua nos modos Operador e Conselheiro
([AGENTS.md](../AGENTS.md)).
