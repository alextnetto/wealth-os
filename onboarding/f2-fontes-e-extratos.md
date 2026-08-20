# F2: Fontes e extratos

**Entrega:** todo lugar onde o dinheiro do titular está ou circula vira uma fonte documentada:
`fontes/<nome>/` com README e `raw/`. Os primeiros 12 meses de dado bruto (ou uma pendência
datada por fonte) entram no repo.

**Pré-requisito:** F1 fechada (`plan/mandato.md` existe). Ao abrir a fase, anuncie onde está e o
que falta ("Estamos na F2, fontes e extratos; faltam os extratos de X e Y"). Onboarding é
retomável por definição; quem anuncia o estado é o agente, não a memória do titular.

**Por que esta fase existe:** o mandato (F1) diz para onde ir; sem dado bruto, o resto é
opinião. Orçamento (F3) e patrimônio (F4) só são confiáveis se derivarem de extrato real. Esta
fase não processa nada; apenas inventaria, documenta e coleta. Nenhum parser é escrito aqui.
Scripts nascem na F3+, contra o dado real: parser escrito contra dado imaginado quebra no
primeiro arquivo real.

## Passo 1: Inventário guiado por classe

Percorra as classes abaixo na ordem, uma pergunta por vez, múltipla escolha quando possível
("seus cartões ativos são: (a) Nubank, (b) Inter, (c) outro; quais?"). Registre a resposta
antes da próxima pergunta. "Não sei / depois" é resposta válida: vira pendência em
`plan/proximos-passos.md` e o inventário segue.

**Registrar a resposta significa materializá-la no repo imediatamente.** Para cada fonte
confirmada, crie na hora `fontes/<nome>/` com o README do template (mesmo que só com o campo
"O que é" preenchido; o Passo 2 completa). Se a fonte for incerta ("acho que tenho um
consórcio"), abra uma linha de pendência em `plan/proximos-passos.md` nomeando a fonte.
Resposta que existe só na conversa não é registro. A lista de fontes declaradas precisa ser
reconstruível do repo por qualquer sessão futura; é essa lista que o gate de saída desta fase
verifica.

| Classe | Pergunta-guia | O que registrar |
|---|---|---|
| Contas bancárias | onde você tem conta que movimenta dinheiro? | banco, tipo, uso (principal/secundária/dormante) |
| Cartões de crédito | quais cartões ativos? | emissor, onde a fatura chega (app/e-mail) |
| Corretoras / B3 | investe em bolsa? por quais corretoras? | corretoras; ativos B3 saem em um export único |
| Cripto | usa exchange? tem carteira própria? | exchanges; endereços públicos por rede |
| Imóveis | próprios, financiados, em obra? | imóvel, situação, quem financia |
| Veículos | carro/moto? financiado? segurado? | veículo, financiamento, apólice |
| PJ própria | tem empresa? | conta PJ, regime, quem faz a contabilidade |
| Previdência / consórcios | PGBL/VGBL? consórcio ativo? | instituição, plano |
| Bens de valor | bens relevantes (sugestão: > R$ 5.000)? | item, nota fiscal se houver |
| Dívidas | empréstimo solto (pessoal, consignado)? | credor, saldo aproximado |

Dívida atada a um bem (financiamento do apartamento ou do carro) fica na fonte do bem; dívida
solta vira fonte própria. Racional: a fonte é a unidade de documentação e coleta; o que se
obtém junto fica registrado junto.

Feche o inventário com a pergunta de varredura: "existe dinheiro seu em algum lugar que não
listamos?". Perguntar agora custa pouco; descobrir uma fonte omitida na F4 custa retrabalho.

## Passo 2: Um diretório por fonte

Para cada fonte declarada, crie `fontes/<nome>/` (kebab-case: `fontes/nubank/`,
`fontes/cartao-inter/`, `fontes/b3/`, `fontes/binance/`) contendo:

- `README.md` a partir de [`../templates/fonte-README.md`](../templates/fonte-README.md):
  o que é o dado, **como obter** (passo a passo), **formato** (raw e consolidado), **como
  processar**, cobertura (de/até), pendências.
- `raw/`: o dado bruto, imutável.

O README é o manual de reposição da fonte: qualquer agente, em qualquer sessão futura, reobtém
e reprocessa o dado sem depender de memória de conversa. `raw/` nunca se edita. Toda camada
consolidada é gerada por script determinístico e 100% reconstruível a partir do raw; se o raw
muda em silêncio, nada mais é auditável. Doutrina completa em
[`../principios/arquitetura.md`](../principios/arquitetura.md).

Carteiras de autocustódia: o README guarda apenas endereços públicos e a rede. Seed, chave
privada e senha não entram no repo, nem em `.env`. Sem exceção. Ver
[`../principios/privacidade.md`](../principios/privacidade.md).

## Passo 3: Pedido de extratos (concreto, por tipo)

Peça **12 meses** (ou o que houver; registre o que veio). O titular exporta. O agente não pede
senha de banco nem opera o internet banking, sem exceção: orienta onde clicar e recebe o
arquivo pronto. Extrato no git do repo privado do titular é o contrato que a F0 verificou.

| Fonte | O que pedir | Formato preferido | Roteiro típico |
|---|---|---|---|
| Conta bancária | extrato do período | CSV ou OFX | app/site → extrato → exportar → escolher período |
| Cartão de crédito | faturas fechadas | CSV se houver; senão PDF | app → faturas → mês a mês → baixar |
| B3 | movimentações + posições | Excel/CSV | Área do Investidor (investidor.b3.com.br) → extratos → movimentação → filtrar → exportar |
| Exchange cripto | histórico de transações | CSV | conta → relatórios/histórico → exportar |
| Carteira própria | endereços públicos | texto no README | copiar o endereço público de cada rede |
| Imóvel | contrato + fluxo de parcelas | PDF + boletos recentes | pasta do contrato, e-mails da incorporadora/banco |
| Veículo | contrato do financiamento, apólice | PDF | banco / seguradora |
| PJ própria | extrato PJ + guias (DAS…) | CSV/OFX + PDF | mesmo roteiro de banco; pedir ao contador |
| Previdência/consórcio | extrato do plano | PDF | portal da instituição |

Prefira CSV ou OFX a PDF: o arquivo será parseado na F3, e PDF exige extração, com um parser
novo por layout. Aceite PDF quando for o único formato (fatura de cartão é o caso comum no
Brasil). Exports com janela máxima (a B3 limita o período por export) saem em fatias: um
arquivo por janela, com o nome codificando o período, sem sobreposição.

## Passo 4: Nomear, guardar, registrar cobertura

- O nome do raw codifica o período coberto: `extrato-2026-01..2026-06.csv`, `fatura-2026-07.pdf`,
  `movimentacoes-2025-07..2026-06.xlsx`. Racional: a cobertura fica auditável no `ls`, sem
  abrir nenhum arquivo.
- Registre no README da fonte a cobertura (de/até) e as lacunas conhecidas.
- **Raw nunca se edita.** Se o arquivo veio errado ou corrompido, reexporte e salve como
  arquivo novo. O antigo só sai com decisão explícita e commit próprio.
- Faça commit por lote de arquivos recebidos. Extrato fora do repo não conta como coletado.

## Pendências

O que o titular não conseguir agora vira linha datada em `plan/proximos-passos.md` (ex.:
"exportar extrato Nubank 12m, até 2026-09-05"). Pendência nunca trava a fase: o sistema
funciona com dado parcial e melhora a cada rotina mensal (F6). O que trava a fase é pendência
não registrada. Silêncio é o único estado proibido.

## Gate de saída

A fase fecha quando:

- [ ] toda fonte declarada no inventário tem `fontes/<nome>/` + README preenchido
- [ ] toda fonte tem raw em `raw/` ou pendência datada em `plan/proximos-passos.md`
- [ ] cobertura (de/até) registrada no README de cada fonte com raw
- [ ] titular confirmou que o inventário está completo

Commit: `f2: fontes e extratos`. Anuncie a próxima fase
([F3: orçamento e recorrentes](f3-orcamento-e-recorrentes.md)) e sugira pausa se a sessão
estiver longa. Fim de fase é o ponto natural de parada.
