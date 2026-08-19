# F1 — Entrevista de mandato

O mandato é o briefing canônico do patrimônio: o documento que qualquer agente lê antes de
analisar ou recomendar qualquer coisa. Ele transforma preferências implícitas ("acho que não
gosto de risco") em política explícita ("cripto ≤ 10%, reserva de 6 meses, dívida sob regra").
Sem mandato, toda recomendação futura precisa re-perguntar tudo; com mandato, o agente trabalha
dentro de limites que o titular definiu em calma — não no calor de uma alta ou de uma queda.

**Entrega da fase:** `plan/mandato.md` preenchido a partir de
[../templates/mandato.md](../templates/mandato.md), status "v1 — provisório até 1º snapshot".
**Gate para F2:** mandato commitado e confirmado pelo titular após leitura de volta.
**Pré-requisito:** F0 fechada (checklist de saída de [f0-preparacao.md](f0-preparacao.md)
completo — em especial `plan/perfil.md`, que dá a moeda-base usada pela F4, e
`plan/ferramentas.md`, que a F5 exige). Se faltar item, volte à F0 antes da entrevista.

## Como conduzir

- **Uma pergunta por vez.** Registre a resposta no rascunho do mandato ANTES da próxima
  pergunta. Racional: bloco de perguntas gera resposta rasa; registro imediato evita perder
  nuance e permite retomar a entrevista de onde parou.
- **Múltipla escolha quando possível.** Opções concretas destravam quem nunca pensou no tema.
- **"Não sei / depois" nunca trava.** Registre um default provisório MARCADO como provisório e
  abra pendência em `plan/proximos-passos.md` com o que muda conforme a resposta.
- **O agente propõe, o titular decide.** Rascunhos e sugestões são seus; o número final é dele.
- Duração típica: 45–60 min. Se o titular cansar, pause no fim de um bloco — a entrevista é
  retomável (os blocos já respondidos estão registrados).

Doutrina completa em [../principios/governanca.md](../principios/governanca.md).

---

## Bloco 1 — O número

**Pergunta:** "Qual é o alvo? Escolha o formato que fizer mais sentido:
(a) patrimônio-alvo — ex.: 'R$ 2.000.000 aos 45';
(b) renda passiva alvo — ex.: 'R$ 8.000/mês a partir de 2040';
(c) ainda não sei — definimos um provisório e refinamos no 1º comitê."

**Por quê:** sem número não há como dimensionar aporte, risco necessário nem prazo — meta vaga
produz carteira vaga e comitê sem critério. O número pode (e deve) ser revisado; o que não pode
é não existir.

**Regra da moeda:** se a meta for em moeda estrangeira, registre o câmbio da data junto
(ex.: "US$ 500.000 em 10 anos — câmbio na data: R$ 5,00"). Racional: sem o câmbio registrado,
a revisão anual não distingue desvio de mercado de desvio de moeda.

**Se a resposta for (c):** registre no mandato "Meta: a definir — provisória", abra pendência
em `plan/proximos-passos.md` com data-limite no 1º comitê e anote que a F3 (renda e sobra) e a
F4 (patrimônio atual) darão a base para você propor um número no 1º comitê (ex.: patrimônio
atual + sobra projetada × horizonte). Não invente um número agora: meta sem base é dado
inventado (regra 6 de [AGENTS.md](../AGENTS.md)).

**Exemplos de resposta:** "R$ 2.000.000 aos 45"; "renda passiva de R$ 8.000/mês em 2040";
"US$ 500.000 em 10 anos (câmbio na data: R$ 5,00)".

## Bloco 2 — Leitura estratégica

**Pergunta:** "No seu horizonte, o que move o patrimônio:
(a) sua carreira ou negócio — a renda cresce mais rápido que qualquer carteira;
(b) a carteira — você já vive (ou quase) do que ela rende;
(c) meio a meio."

**Por quê:** o papel da carteira decorre desta resposta, e quase todo erro de alocação vem de
ignorá-la. Se o motor é carreira/negócio, a carteira serve para **proteger e compor** o que o
trabalho gera — não faz sentido correr risco de ruína para "multiplicar" o que a renda
multiplica mais rápido. Se o motor é a carteira, ela precisa **gerar e sustentar**, e a
política de risco vira o centro do mandato. Nenhuma alocação líquida multiplica muitas vezes
em poucos anos sem risco desproporcional — quem promete isso está vendendo algo.

**Exemplos de resposta:** "assalariado com renda crescente; motor é a carreira → carteira
protege e compõe"; "vendi minha participação, vivo da carteira → papel é renda com
preservação".

## Bloco 3 — Marcos de vida

**Pergunta:** "Há marcos com impacto financeiro relevante à frente — comprar ou trocar de
moradia, filhos, sabático, mudança de país, apoio a familiares? Para cada um: com data
aproximada, ou 'sem data'."

**Por quê:** marco datado é um passivo de liquidez — dinheiro com data marcada não pode estar
em renda variável, e isso subordina a alocação do Bloco 8. "Sem data" também é resposta útil:
registrada, sai da cabeça do titular e entra na pauta da revisão anual em vez de virar decisão
de impulso.

**Exemplos de resposta:** "entrada de imóvel, ~R$ 200.000, em 2028"; "filho: sem data";
"sabático de 6 meses em 2027, ~R$ 60.000"; "nenhum marco planejado".

## Bloco 4 — Perfil de risco

**Pergunta (em duas partes):** "Você já viu um investimento seu cair mais de 30%?
Se sim: o que você FEZ — segurou, vendeu, comprou mais?
Se nunca: imagine a carteira 35% abaixo do topo por um ano — qual seria sua reação honesta?"

**Por quê:** autodeclaração de tolerância a risco vale pouco — quase todo mundo se declara
"moderado" em mercado calmo. A única evidência confiável é comportamento em drawdown real:
quem segurou (ou comprou) tem tolerância comprovada; quem vendeu no fundo tem perfil
conservador na prática, independente do que declare. Sem histórico, o perfil é **não testado**
e o mandato começa mais conservador até haver evidência — subir o risco depois é barato;
descobrir o perfil real vendendo no fundo é caro.

**Exemplos de resposta:** "carteira caiu ~40% em 2022, segurei sem vender → tolerância
comprovada"; "vendi tudo numa queda de 30% → perfil real: conservador"; "nunca passei por
queda relevante → não testado, começar conservador".

## Bloco 5 — Liquidez mínima

**Pergunta (em duas partes):** "Qual seu custo fixo mensal aproximado — moradia, contas,
prestações, tudo que sai todo mês? E quantos meses desse custo você quer líquidos como reserva
de emergência: 3, 6 ou 12?"

**Por quê:** a reserva existe para impedir venda forçada em crise — o maior destruidor de
retorno composto é ser obrigado a vender no fundo para pagar boleto. A régua depende da
estabilidade da renda: CLT estável → 3–6 meses; renda variável, PJ ou autônomo → 6–12. O custo
fixo dado aqui é aproximado de propósito: a F3 o refina com dados reais e o mandato é
atualizado se a diferença for relevante.

**Exemplos de resposta:** "custo fixo ~R$ 8.000/mês; 6 meses → reserva de R$ 48.000 em
RF/caixa de resgate imediato"; "sou PJ, renda variável → 12 meses".

## Bloco 6 — Tetos e vetos

**Pergunta (em duas partes):** "Alguma classe deve ter teto máximo — ex.: 'cripto no máximo
10% do patrimônio'? E há vetos — coisas em que você não investe de jeito nenhum?"

**Por quê:** teto declarado em calma vale mais que disciplina prometida em euforia — o agente
usa os tetos como guarda-corpo em TODA recomendação futura, sem precisar rediscutir. Veto é
veto: não volta como "oportunidade imperdível" em análise nenhuma. "Sem tetos" e "sem vetos"
também se registram — resposta explícita fecha a discussão; ausência de resposta a reabre a
cada análise.

**Exemplos de resposta:** "cripto ≤ 10% do total; imobiliário ≤ 40%"; "vetos: derivativos
alavancados, day trade, empréstimo a conhecidos"; "sem vetos".

## Bloco 7 — Alavancagem

**Pergunta:** "Qual sua relação com dívida?
(a) bem-vinda sob regras — quais limites?
(b) tolerada — só a que já existe, nada novo;
(c) proibida — zero dívida é objetivo."

**Por quê:** alavancagem sem regra escrita é decidida no impulso — na oferta do gerente, no
plantão de vendas do imóvel. Regra escrita antes da oferta é a única defesa. Regras típicas em
dois eixos, mais uma trava de processo:

- **Fluxo:** serviço total da dívida (soma de todas as prestações) ≤ X% da renda mensal
  recorrente — ex.: 25%. Protege o caixa do mês.
- **Estoque:** dívida total ≤ Y% do patrimônio bruto — ex.: 30%. Protege o balanço.
- **Processo:** toda alavancagem NOVA passa por análise formal com recomendação e vira decisão
  registrada em `plan/decisions.md` — nunca impulso, qualquer que seja a taxa.

**Exemplos de resposta:** "bem-vinda: serviço ≤ 25% da renda, dívida ≤ 30% do patrimônio
bruto, análise formal sempre"; "só o financiamento atual do apartamento; nada novo";
"proibida — quitar tudo é meta".

## Bloco 8 — IPS: alocação-alvo com bandas

**Pergunta:** "Como você quer o patrimônio dividido por classe? Para cada classe: alvo em % e
banda de tolerância. Se nunca pensou nisso, eu proponho um rascunho conservador e marcamos
como provisório."

**Por quê:** o IPS (Investment Policy Statement) transforma sensação ("acho que tenho renda
fixa demais") em teste objetivo: dentro ou fora da banda. A banda existe para não rebalancear
a cada ruído de mercado — só desvio ALÉM da banda vira pauta obrigatória do próximo comitê, e
mesmo aí a resposta é proposta de rebalanceamento, **nunca execução automática**.

Se o titular não souber, proponha um rascunho conservador como este (valores fictícios —
ajustar aos blocos 3–7) e MARQUE como provisório:

| Classe | Alvo | Banda | Notas |
|---|---|---|---|
| RF / caixa | 40% | ±10pp | inclui a reserva de emergência (Bloco 5) |
| Ações BR | 20% | ±5pp | |
| Ações globais | 15% | ±5pp | |
| Imobiliário (FIIs) | 15% | ±5pp | |
| Cripto | 5% | ±5pp | teto do Bloco 6 prevalece sobre a banda |
| Outros (bens, veículos) | 5% | residual | sem banda |

**Regra do provisório:** qualquer IPS desenhado antes do snapshot real é hipótese. Marque
"provisório até o 1º snapshot (F4)": a F4 mostra a distância entre alvo e realidade, e o
1º comitê recalibra com o titular — nunca silenciosamente.

## Bloco 9 — Política de aportes

**Pergunta (em duas partes):** "Quanto você se compromete a aportar por mês, no mínimo — um
número que sobrevive a mês ruim? E o que CONTA como aporte: só dinheiro novo em investimentos,
ou também amortização de dívida e parcela de imóvel?"

**Por quê:** o aporte mínimo é a variável mais controlável do plano inteiro — mais que
retorno, mais que alocação. Melhor um mínimo defensável cumprido todo mês que um ideal furado.
Definir o que conta evita contabilidade criativa nos dois sentidos: amortizar dívida cara é
alocação de capital (não despesa), e se contar como aporte, a comparação amortizar × investir
entra como avaliação recorrente da rotina — não decisão pontual.

**Exemplos de resposta:** "R$ 3.000/mês, só investimentos líquidos"; "R$ 5.000/mês, contando
amortização do financiamento como aporte".

## Bloco 10 — Cadências

**Pergunta:** "Confirmando as cadências do sistema: rotina mensal no dia `<dia do
plan/perfil.md>`, comitê trimestral, revisão anual do mandato — ajusta algo?"

**Por quê:** sistema sem cadência vira planilha morta em três meses. Cada camada tem função
distinta: **rotina mensal** (30–45 min) é operação — classificar, dar baixa, republicar;
**comitê trimestral** é decisão — desvios de banda, riscos, análises, ata em
`plan/decisions.md`; **revisão anual** é o único momento em que o próprio mandato muda fora de
evento de vida. O dia da rotina veio da F0 (sugestão: dia 5 — fecha o mês anterior antes do
bloco de vencimentos do dia 10).

**Exemplos de resposta:** "rotina dia 5; comitês em janeiro, abril, julho e outubro; revisão
do mandato todo agosto".

---

## Saída da fase

1. **Preencher `plan/mandato.md`** a partir de
   [../templates/mandato.md](../templates/mandato.md) — o template espelha os 10 blocos.
   Primeira linha de status: **"v1 — provisório até 1º snapshot"**. Racional: o mandato só
   deixa de ser provisório quando a F4 confrontar a política com o patrimônio real.
2. **Ler de volta:** resuma o mandato ao titular em 5–8 linhas (número, motor, reserva, tetos,
   regra de dívida, alvo de alocação, aporte, cadências) e peça confirmação explícita.
   Racional: o titular precisa reconhecer o documento como DELE — mandato não confirmado não
   governa nada.
3. **Pendências:** todo "não sei / depois" da entrevista deve estar em
   `plan/proximos-passos.md` com data-limite e o que muda conforme a resposta.
4. **Commit:** `f1: mandato v1`.

Checklist de saída (gate):

- [ ] Os 10 blocos respondidos ou com default provisório marcado + pendência aberta
- [ ] Câmbio registrado, se a meta for em moeda estrangeira
- [ ] IPS com tabela alvo/banda/notas (marcado provisório se rascunho do agente)
- [ ] `plan/mandato.md` com status "v1 — provisório até 1º snapshot"
- [ ] Titular confirmou o resumo lido de volta
- [ ] Commit `f1: mandato v1` feito

Próxima fase: [f2-fontes-e-extratos.md](f2-fontes-e-extratos.md) — inventário de contas e
pedido de extratos. Boa pausa natural: o mandato fecha uma sessão inteira com valor entregue.
