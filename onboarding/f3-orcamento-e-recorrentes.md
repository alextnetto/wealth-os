# F3: Orçamento e recorrentes

**Entrega:** `plan/orcamento.csv` (renda + fixos), `plan/recorrentes.csv` (obrigações datadas),
`plan/categorias.csv` (taxonomia de gastos) e, se houver extrato real da F2,
`plan/regras-classificacao.csv`, `plan/excecoes.csv` e a primeira classificação com o retrato
de gastos. Fecha com o primeiro fluxo de caixa simples: entradas − fixos − recorrentes −
variável = sobra.

**Pré-requisito:** F2 fechada (toda fonte declarada tem dir + README + raw ou pendência). Ao
abrir, anuncie a fase e o que falta.

**Por que:** memória subestima gasto; extrato não. A entrevista dá a base do orçamento; o dado
real da F2 corrige a estimativa. Por isso a ordem é entrevista primeiro (custo baixo, sempre
possível) e classificação depois (custo alto, exige raw). O gate da fase mede a classificação,
não a opinião. Como em toda entrevista: uma pergunta por vez, múltipla escolha quando possível,
registrar a resposta antes da próxima.

## Passo 1: Entrevista de renda

Fontes típicas: salário CLT, pró-labore, distribuição de lucros, renda variável (freelance,
comissão, bônus), aluguéis, rendimentos recorrentes. Por fonte: valor **líquido**, moeda,
frequência. Renda variável entra pela estimativa **conservadora** (piso ou mediana dos últimos
12 meses), com o critério na `obs`. Orçamento otimista é o erro que o sistema existe para
impedir. Renda em moeda estrangeira fica na moeda original; a conversão pertence à camada
consolidada, não ao registro.

A saída é `plan/orcamento.csv`. Schema (contrato, idêntico a `templates/orcamento.csv`):

```
tipo,categoria,valor,moeda,freq,meses,obs
```

- `tipo` ∈ `entrada` | `saida`
- `freq` ∈ `mensal` | `anual`; quando `anual`, `meses` indica em quais meses ocorre
  (`6;12` = junho e dezembro)
- exemplo (fictício): `entrada,salario,10000,BRL,mensal,,liquido apos IR e INSS`

## Passo 2: Fixos fora de cartão

Saídas que não passam pela fatura: moradia (aluguel, condomínio), prestadores fixos pagos por
Pix/transferência (diarista, personal, terapeuta), mensalidades em débito automático (escola,
academia), pensão. Entram no mesmo `plan/orcamento.csv` com `tipo=saida`.

**Regra do não-recontar:** prestador fixo pago por Pix recebe `pago-por-pix` na `obs`. Quando a
classificação de extratos rodar (passo 5), cada um vira linha em `plan/excecoes.csv` com o
grupo-sentinela `JA_CONTADO`. Racional: o Pix aparece no extrato da conta; sem a exceção, o
mesmo prestador conta duas vezes, como fixo do orçamento e como gasto variável do ledger, e a
sobra projetada sai menor que a real.

## Passo 3: Obrigações datadas

Toda obrigação com data de vencimento e consequência em caso de atraso entra em
`plan/recorrentes.csv`. Este arquivo gera a agenda de vencimentos (F5) e as linhas de
obrigações do fluxo de caixa; por isso precisa ser expansível por script, não texto solto.
Casos típicos BR: financiamento imobiliário, IPTU, IPVA, licenciamento, seguros, DAS (se há
PJ), anuidades.

Schema (contrato, idêntico a `templates/recorrentes.csv`):

```
id,descricao,classe,freq,dia,meses,parcelas,valor_estimado,indexador,inicio,fim,fonte,obs
```

| Campo | Regra |
|---|---|
| `id` | kebab-case estável (`ipva-carro`, `financiamento-apto`). É proibido renomear: a agenda (F5) referencia por id via `plan/agenda-map.csv` |
| `classe` | natureza da obrigação: `financiamento`, `imposto`, `seguro`, `anuidade`, `assinatura`… |
| `freq` | `mensal` \| `anual` \| `unica` (evento com data única, ex.: entrega de chaves) |
| `dia` | dia do mês do vencimento |
| `meses` | para `anual` ou parcelado em meses fixos: `1;2;3` = janeiro, fevereiro, março |
| `parcelas` | total de parcelas quando finito (`120`); vazio = sem fim definido |
| `valor_estimado` | valor da parcela/guia hoje; estimativa serve, a rotina mensal (F6) corrige |
| `indexador` | o que corrige o valor: `fixo`, `IPCA`, `TR`, `SELIC`… |
| `inicio` / `fim` | `YYYY-MM`; `fim` vazio = perpétua |
| `fonte` | caminho que comprova (`fontes/financiamento-apto/README.md`) |

Exemplo (fictício, o mesmo de `templates/recorrentes.csv`):
`ipva-carro,IPVA do carro em 3 cotas,imposto,anual,10,1;2;3,3,400,fixo,2026-01,,fontes/carro/README.md,`

## Passo 4: Taxonomia de gastos

Copie `templates/categorias.csv` para `plan/categorias.csv`. O template já traz a taxonomia
inicial genérica BR (~9 grupos × ~25 subcategorias); ajuste com o titular apenas o que a vida
dele exigir. Taxonomia enxuta que classifica é melhor que taxonomia completa que confunde.

Schema (contrato): `ordem,grupo,subcategoria,estimador,meta_mensal,obs`

- `estimador` ∈ `mediana12m` | `media12m`, sempre com a janela no nome. "Média" sem janela é
  ambígua e gera divergência de interpretação depois.
- `meta_mensal` fica vazia por enquanto (passo 6)

**Append-only:** categoria nova entra no fim (próxima `ordem`). É proibido reordenar ou
remover. Racional: `ordem` é identidade estável; regras e histórico referenciam a taxonomia, e
reordenar reescreve o passado em silêncio.

## Passo 5: Classificação (com extrato real)

Se a F2 trouxe raw de cartão ou conta, o agente escreve agora o primeiro classificador, contra
o dado real, nunca antes: parser contra dado imaginado quebra no primeiro arquivo real. Todo
script: determinístico, idempotente, com teste, falha ruidosa. Doutrina em
[`../principios/arquitetura.md`](../principios/arquitetura.md).

**Artefato de saída: o ledger da fonte.** A classificação grava `fontes/<fonte>/ledger.csv`
(um por fonte com raw), gerado por script determinístico a partir de `raw/` + regras. Schema
mínimo (contrato, definido aqui e citado pela F5 e pela F6): `data,descricao,valor,grupo,subcategoria,origem_raw,status`,
com `status ∈ previsto|pago`. É nesta coluna que a rotina mensal (F6) dá as baixas. O ledger é
derivado e 100% reconstruível: apagar e reclassificar produz o mesmo arquivo, exceto `status`,
que é estado da fonte e se preserva por chave `data+descricao+valor`. A F5 consome os ledgers
ao consolidar obrigações e o Orçado × Realizado.

**Precedência (a primeira camada que casa vence):**

```
exceções  >  regex  >  de-para  >  fallback diversos/a-classificar
```

1. **Exceções**: `plan/excecoes.csv`, schema (contrato):
   `fonte,data,padrao_descricao,grupo,subcategoria,obs`. Decisões pontuais por transação ou por
   prestador conhecido. `padrao_descricao` é **regex case-insensitive, não substring literal**:
   grafias variam no extrato (`Jo[aã]o` casa com e sem acento). Tratar a coluna como substring
   faz a exceção falhar em silêncio: nenhum erro, só recontagem. Grupos-sentinela documentados:
   `JA_CONTADO` (já é fixo do orçamento; não recontar no ledger, `subcategoria` vazia) e
   `ENTRADA` (é receita no meio do extrato; sai do ledger de gastos). **Semântica da `data`:**
   preenchida = exceção pontual, o casamento exige data + regex (uma compra específica);
   vazia = exceção permanente por prestador, casa qualquer data, só pela regex. A data de
   criação da regra, se relevante, vai na `obs`.
2. **Regex**: `plan/regras-classificacao.csv`, schema (contrato):
   `ordem,fonte,padrao_regex,grupo,subcategoria,obs`. Regex case-insensitive sobre a descrição
   da transação. **Regra da âncora: ancore no nome do comerciante, nunca no gateway/adquirente**
   (Cielo, Stone, PagSeguro, Zig…). Racional: um adquirente processa pagamentos de comerciantes
   de qualquer categoria. Padrão ancorado no gateway classifica restaurante como lazer e
   farmácia como viagem, em massa e em silêncio.
3. **De-para**: quando a fonte já traz categoria própria (fatura de banco costuma vir com
   "mercado", "restaurante"…), mapeie a categoria da fonte para grupo/subcategoria da taxonomia
   em `plan/depara-categorias.csv`, header `fonte,categoria_fonte,grupo,subcategoria`, apontando
   para o destino dominante da categoria de origem (a minoria se recorta com regex, na camada
   acima). O arquivo nasce junto com o classificador, a partir do vocabulário real da fonte.
   Não existe template dele porque não é possível mapear categorias ainda não observadas.
4. **Fallback**: `diversos/a-classificar`. É proibido atribuir categoria sem regra: transação
   sem regra fica a-classificar e, se relevante, vira pergunta ao titular. Dado inventado é
   pior que dado faltando: o dado faltando se corrige, o inventado se esconde.

## Passo 6: Metas por subcategoria

Onde houver 12 meses de histórico classificado: **meta = mediana dos 12 meses, arredondada para
cima em múltiplo de R$ 50**. Racional: a mediana ignora o mês fora do padrão (a viagem, o
conserto); arredondar para cima evita meta descumprida desde o primeiro mês; múltiplo de 50 é
legível. Sem histórico, `meta_mensal` fica vazia, com o motivo na `obs`. Meta sem dado é
estimativa sem base.

A meta calculada é uma proposta: apresente a tabela, o titular decide linha a linha (aceitar,
ajustar, adiar) e o aceite fica registrado.

## Passo 7: Retrato e gate

Com a classificação rodada, apresente o retrato: gasto médio mensal, top 5 categorias por valor,
% a-classificar no último mês fechado.

**Gate: `a-classificar` < 10% do valor no último mês fechado.** Acima disso, o orçamento não
reflete o gasto real. Para baixar: liste os maiores itens não classificados por valor (não por
contagem: um boleto de valor alto pesa mais que trinta compras pequenas) e pergunte ao titular
um a um. Cada resposta vira exceção ou regra nova, e a regra nova respeita a âncora no
comerciante.

Sem extrato real (a F2 fechou só com pendências): os passos 5 a 7 viram pendência datada em
`plan/proximos-passos.md` e o orçamento nasce só da entrevista, marcado "estimado, sem
histórico". O gate é cobrado na primeira rotina mensal (F6). Nesse caso, pergunte ao titular
quanto ele estima gastar por mês em variáveis (cartão + dia a dia), registre em
`plan/orcamento.csv` como `saida,variavel-estimado,<valor>,BRL,mensal,,estimativa do titular sem historico`
e abra pendência para substituir pela média real quando o raw chegar. Sem esse número, o fluxo
de caixa do passo 8 (e o da F5) não fecha.

## Passo 8: Fluxo de caixa simples

Com orçamento e recorrentes prontos, apresente a conta:

```
entradas − fixos − recorrentes médios − variável estimado = sobra projetada
```

**Variável estimado** (definição; a F5 usa a mesma): gasto médio mensal classificado dos
últimos meses fechados, excluindo linhas `JA_CONTADO` e `ENTRADA`, quando a classificação rodou
(passo 5); sem extrato, é a estimativa declarada pelo titular (linha `variavel-estimado` do
orçamento, passo 7).

Exemplo fictício: 12.000 − 4.500 − 1.000 − 3.500 = **R$ 3.000/mês de sobra**. É conta derivada
dos CSVs, apresentada em conversa. Não vira arquivo aqui: a projeção materializada (24 meses)
nasce na F5, gerada por script a partir dos mesmos CSVs. Nada é digitado duas vezes.

Se a sobra projetada ficar abaixo do aporte mínimo do mandato (F1, política de aportes),
informe o titular nesta fase e registre como pauta. É conflito entre plano e realidade, não
descoberta para o 1º comitê.

## Gate de saída

- [ ] `plan/orcamento.csv` com todas as entradas e fixos declarados (Pix fixo marcado)
- [ ] `plan/recorrentes.csv` com toda obrigação datada conhecida, `fonte` preenchida
- [ ] `plan/categorias.csv` copiado do template e ajustado com o titular
- [ ] classificação rodada com a-classificar < 10% no último mês fechado, ou pendência datada
- [ ] metas propostas e decididas pelo titular (aceitas, ajustadas ou adiadas, com registro)
- [ ] sobra projetada apresentada e confrontada com o mandato

Commit: `f3: orçamento e recorrentes`. Anuncie a próxima fase
([F4: snapshot de patrimônio](f4-patrimonio.md)).
