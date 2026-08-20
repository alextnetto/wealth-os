# F4: Snapshot de patrimônio

**Entrada:** F3 fechada (orçamento, recorrentes e taxonomia em `plan/`). **Saída:**
`plan/patrimonio.csv` (snapshot datado) + `plan/saldos-manuais.csv` + patrimônio líquido calculado
e comparado com o IPS do mandato. **Gate:** toda fonte declarada na F2 tem linha no snapshot ou
pendência registrada; a comparação com o IPS foi apresentada ao titular.

## Objetivo

Levantar o primeiro balanço completo: uma linha por item, ativos e passivos, todos na mesma data.
Este número alimenta o resto do sistema: a aba Patrimônio (F5), o drift contra o IPS e o comitê
trimestral (F6). Sem um snapshot fiel, a política de alocação do mandato não tem base de
verificação.

Princípios que regem a fase:

- **Snapshot é datado e append-only.** `plan/patrimonio.csv` acumula linhas por data: a rotina
  mensal (F6) acrescenta um snapshot novo, nunca edita um antigo. Racional: a série histórica
  permite ver evolução e drift; editar o passado destrói as duas leituras.
- **Célula sem fonte é célula vazia + pendência.** Não estime saldo sem fonte para fechar o
  balanço. Um número inventado hoje vira decisão errada no comitê. O que faltar entra em
  `plan/proximos-passos.md` com data-limite.
- **Uma pergunta por vez.** Ao pedir saldos ao titular, pergunte um item, registre, só então
  pergunte o próximo (doutrina do [protocolo](README.md)).

## Regras de valoração por classe

| Classe | Como valorar | Fonte do dado | Racional |
|---|---|---|---|
| contas | saldo na data | extrato (F2) ou app do banco | dado direto, sem estimativa |
| bolsa | posições × cotação da data | export da Área do Investidor B3 | posição oficial, não a memória |
| cripto | posições por endereço público × cotação | explorador público / export da exchange | só endereços **públicos** entram no repo; nunca seed, chave ou senha ([privacidade](../principios/privacidade.md)) |
| imoveis | **a custo**: aquisição + benfeitorias documentadas | escritura, contratos, NFs | regra conservadora: valor de mercado sem liquidez infla o PL e mascara drift; mercado só com avaliação formal, registrada como decisão |
| veiculos | tabela FIPE do mês | consulta FIPE pela placa/modelo | referência pública e reproduzível |
| bens | valor conservador; só itens acima do corte (sugestão: R$ 5.000) | NF ou estimativa declarada | inventariar itens de pequeno valor custa mais do que informa |
| previdencia | valor de resgate na data | app/extrato da seguradora | o valor de resgate existe na data; a projeção não |
| pj | tesouraria (caixa + aplicações) × participação do titular | extrato PJ (F2) | ver regra da PJ abaixo |
| passivos | **saldo devedor com sinal negativo** | contrato/app do credor | ver regra dos passivos abaixo |

**Passivos entram pelo saldo devedor, não pela parcela.** A parcela é fluxo (já está em
`plan/recorrentes.csv`, F3); o que reduz o patrimônio é o estoque da dívida. Registre o valor
negativo: o patrimônio líquido sai da soma simples da coluna, sem coluna de sinal separada.

**PJ própria: só a tesouraria, ponderada pela participação.** Titular com 100% da empresa soma o
caixa PJ integral; 50% soma metade. O valuation da operação fica fora do snapshot v1: valuation
de empresa própria é subjetivo, infla o PL e não tem liquidez. Se o titular quiser esse número,
ele entra como análise formal (F6), não como linha de balanço.

**Moeda-base sempre.** `patrimonio.csv` não tem coluna de moeda: todo valor entra na moeda-base
de `plan/perfil.md`. Converta item em moeda estrangeira pelo câmbio da data do snapshot e
registre o câmbio usado em `obs`. Racional: balanço em moeda mista não soma.

## Coleta

1. Percorra `fontes/` (inventário da F2). Para cada fonte, derive o valor do raw mais recente
   quando ele existir (extrato, export B3, CSV de exchange). Nunca edite o raw.
2. O que o dado bruto não cobre (previdência sem export, imóvel, veículo, bem, saldo informado
   verbalmente pelo titular) entra em `plan/saldos-manuais.csv` com **fonte e `data_ref`**
   obrigatórias. Sem esses dois campos a linha não vale: vira pendência.
3. Pergunte ao titular item a item o que faltar. Aceite "não sei / depois": registre a pendência
   em `plan/proximos-passos.md` com data-limite e siga. Pendência não trava a fase; linha
   inventada trava.

### Schemas (contrato, idênticos a `templates/`)

`plan/patrimonio.csv`: `data,classe,mundo,item,instituicao,valor,obs` · `mundo ∈ pf|pj` ·
passivo negativo. Exemplo (valores fictícios):

```csv
data,classe,mundo,item,instituicao,valor,obs
2026-09-05,contas,pf,conta-corrente,Nubank,12000,saldo em 2026-09-05
2026-09-05,bolsa,pf,acoes-e-fiis,B3,180000,export area do investidor de 2026-09-04
2026-09-05,cripto,pf,carteira-fria,autocustodia,25000,enderecos publicos; cotacao 2026-09-05
2026-09-05,imoveis,pf,apartamento,-,400000,a CUSTO: escritura + benfeitorias com NF
2026-09-05,veiculos,pf,carro,-,60000,FIPE 2026-09
2026-09-05,pj,pj,tesouraria,Inter,50000,caixa PJ x 100% de participacao
2026-09-05,passivos,pf,financiamento-imovel,Inter,-250000,saldo devedor; nao a parcela
```

`plan/saldos-manuais.csv`: `item,classe,instituicao,valor,moeda,data_ref,fonte,obs`. Exemplo:

```csv
item,classe,instituicao,valor,moeda,data_ref,fonte,obs
previdencia-pgbl,previdencia,Icatu,30000,BRL,2026-09-03,app da seguradora,valor de resgate
```

Copie os templates (`templates/patrimonio.csv`, `templates/saldos-manuais.csv`) para `plan/` e
substitua as linhas de exemplo por dados reais. Exemplo fictício não fica no repo do titular.

## Patrimônio líquido e comparação com o IPS

1. **PL = soma da coluna `valor`** do snapshot da data (passivos já são negativos). Apresente ao
   titular: PL total, ativos brutos, passivos e a quebra por classe.
2. **Compare com o IPS do mandato** (`plan/mandato.md`, bloco 8). Percentuais por classe são
   calculados sobre os **ativos brutos** (soma dos positivos), não sobre o PL. A banda de
   alocação indica onde o capital está; a dívida é regida pelas regras de alavancagem do mandato
   (bloco 7). Se o snapshot é mais granular que o IPS, agregue as classes na comparação.
3. Monte a tabela atual × alvo × banda. Exemplo (fictício):

| Classe (IPS) | Atual | Alvo | Banda | Situação |
|---|---:|---:|---|---|
| RF/caixa | 25% | 20% | ±5 p.p. | dentro |
| Bolsa | 15% | 30% | ±5 p.p. | **fora (−15 p.p.)** → pauta do 1º comitê |
| Cripto | 10% | 10% | ±5 p.p. | dentro |
| Imobiliário | 45% | 35% | ±10 p.p. | dentro |
| PJ | 5% | 5% | ±5 p.p. | dentro |

4. **Desvio fora da banda vira pauta do 1º comitê; rebalanceamento automático é proibido.**
   Registre cada desvio como item numerado em `plan/proximos-passos.md` ("pauta do 1º comitê: …").
   O agente propõe; o titular decide, e decide no comitê (F6). Racional: decisão patrimonial
   sobre o primeiro snapshot, sem análise, é o improviso que o sistema existe para evitar.
5. **Se o IPS era provisório** (marcado assim na F1 porque o titular não sabia a alocação-alvo),
   recalibre agora: apresente a alocação real e pergunte, bloco a bloco, se o alvo proposto
   continua válido. Ajuste aceito vira registro em `plan/decisions.md` (decisão, racional, dados
   no momento).
6. **Remova a marca "provisório" ao fim da comparação, sem exceção.** Vale para os dois caminhos:
   IPS recalibrado (item 5) ou IPS que o titular já tinha definido na F1. No segundo caso,
   confirme com o titular que os alvos seguem válidos diante da alocação real e registre a
   confirmação em `plan/decisions.md`. Atualize a linha de status de `plan/mandato.md` de
   "v1: provisório até o 1º snapshot de patrimônio (F4)" para
   "v1: confirmado no 1º snapshot (<AAAA-MM-DD>)". Racional: o gate de aceite da F6 exige
   mandato sem a marca. Mandato permanentemente provisório não governa.

## Fechamento da fase

- [ ] `plan/patrimonio.csv` com snapshot datado: toda fonte da F2 tem linha ou pendência
- [ ] `plan/saldos-manuais.csv` sem linha órfã (toda linha tem fonte + data_ref)
- [ ] PL apresentado ao titular; comparação com IPS feita; desvios registrados como pauta
- [ ] IPS recalibrado se era provisório (com decisão registrada)
- [ ] Status do mandato atualizado: marca "provisório até o 1º snapshot" removida após a
      comparação (recalibrada ou confirmada pelo titular)
- [ ] Commit: `f4: snapshot de patrimônio`

Próxima fase: [f5-planilha-e-agenda.md](f5-planilha-e-agenda.md), para publicar os espelhos.
