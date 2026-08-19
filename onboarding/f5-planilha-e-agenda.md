# F5 — Planilha mestra e agenda

**Entrada:** F4 fechada (snapshot em `plan/patrimonio.csv`) e rota de integração registrada em
`plan/ferramentas.md` (F0): `gog`, MCP ou fallback local. **Saída:** planilha mestra com 6 abas
(ou fallback CSV+HTML) + agenda "Finanças" com alertas + `plan/planilha-id.txt` +
`plan/agenda-map.csv`. **Gate:** publicação testada como regenerável e idempotente.

## Objetivo e doutrina

O repo é a fonte da verdade; planilha e agenda são **espelhos publicados** — a superfície de
consumo no celular. Duas regras inegociáveis:

- **Mão única repo→Sheets.** O agente nunca lê dado de volta da planilha. Edição feita na
  planilha é perdida na próxima publicação — avise o titular disso ao entregar. Racional: duas
  fontes de verdade divergem em silêncio; uma só, com espelho descartável, não.
- **Publicação regenerável.** Apagar a planilha inteira e re-publicar produz o mesmo resultado.
  Se a publicação depende de estado que não está no repo, ela está errada. (Exceção documentada:
  `plan/agenda-map.csv` guarda IDs que vêm do Google — é estado da publicação, commitado, e a
  única peça não reconstruível do zero.)

Nota de privacidade: publicar no Google significa que um espelho dos dados sai do repo. Isso foi
acordado na F0 e está em `plan/ferramentas.md`; na dúvida, releia
[principios/privacidade.md](../principios/privacidade.md) com o titular antes de publicar.

## Passo 1 — consolidar antes de publicar

A planilha é gerada de camadas derivadas, nunca de conta feita "de cabeça". Gere
`plan/consolidado/` a partir do que as fases anteriores produziram (se o consolidador ainda não
existe, escreva-o agora seguindo [principios/arquitetura.md](../principios/arquitetura.md):
determinístico, idempotente, falha ruidosa, com teste — e nascido do dado real deste repo):

- `plan/consolidado/obrigacoes.csv` — 18 meses à frente, expandindo `plan/recorrentes.csv` em
  ocorrências datadas + parcelas datadas dos ledgers das fontes (`fontes/<fonte>/ledger.csv`,
  schema definido na F3, passo 5). Schema recomendado:
  `obrigacao_id,data_venc,descricao,classe,origem,valor,tipo_valor,status`
  (`origem` = `recorrente:<id>` ou `ledger:<path>`; `tipo_valor ∈ real|estimado`;
  `status ∈ previsto|pago` — baixa acontece na fonte, nunca no consolidado).
- `plan/consolidado/fluxo-caixa.csv` — 24 meses: entradas do orçamento − obrigações do mês −
  variável estimado (definido na F3, passo 8) = sobra/déficit projetado.

**`obrigacao_id` é determinístico**: derive de origem + data (ex.: `ipva-carro:2027-01`), nunca
de contador ou timestamp. Racional: é a chave do `agenda-map.csv` — se o ID muda entre
execuções, a idempotência quebra e a agenda duplica.

## Passo 2 — planilha mestra (6 abas)

| Aba | Conteúdo | Fonte no repo |
|---|---|---|
| Visão Geral | patrimônio líquido, vencimentos 30d, alertas (vencido/hoje/semana), pendências abertas | `patrimonio.csv`, `consolidado/obrigacoes.csv`, `proximos-passos.md` |
| Obrigações | 18 meses à frente: data, descrição, valor, origem, status | `consolidado/obrigacoes.csv` |
| Fluxo de Caixa | 24 meses: entradas − fixos − obrigações do mês − variável = sobra/déficit | `consolidado/fluxo-caixa.csv` |
| Patrimônio | por classe: atual × alvo × banda, drift destacado | `patrimonio.csv` + IPS de `mandato.md` |
| Orçado × Realizado | meta mensal por subcategoria vs realizado do cartão | `categorias.csv` + ledgers das fontes (`fontes/<fonte>/ledger.csv`, F3) |
| Riscos | espelho de `plan/riscos.md` | `riscos.md` (nasce vazio — semeado na F6; re-publicar depois) |

### Rota `gog` (CLI)

A sequência abaixo é o **roteiro, não a sintaxe**. Flags e subcomandos variam entre versões do
`gog`: rode `gog sheets --help` (e o `--help` de cada subcomando) ANTES de executar e adapte.
**Nunca invente flag** — comando que falhar por sintaxe manda reler o help, não chutar variação.

```
gog sheets --help                    # sempre primeiro: confirme subcomandos e flags desta versão
gog sheets create ...                # criar "wealth-os — planilha mestra"; capture o ID da saída
gog sheets values update ...         # escrever cada aba a partir do CSV consolidado
# formatação mínima: header em negrito/congelado, colunas de valor como moeda — só se o
# subcomando existir nesta versão; formatação nunca justifica inventar sintaxe
```

Guarde o ID retornado em `plan/planilha-id.txt` (uma linha, só o ID) e commite. Toda publicação
futura lê o ID daí — se o arquivo não existe, cria planilha nova; se existe, atualiza a mesma.

### Rota MCP

Equivalente: criar a spreadsheet, escrever os ranges de cada aba a partir dos mesmos CSVs
consolidados, guardar o ID em `plan/planilha-id.txt`. As 6 abas e a regra de regenerabilidade
são idênticas — a ferramenta muda, o contrato não.

### Rota sem Google (fallback local)

Gere as mesmas 6 visões em `plan/consolidado/*.csv` + um `plan/consolidado/dashboard.html`
local, auto-contido, que o titular abre no navegador. Gerado por script, nunca editado à mão,
100% reconstruível. O dashboard cumpre o papel da planilha; a limitação real do fallback são os
alertas de calendário (ver abaixo).

## Passo 3 — agenda "Finanças"

1. **Crie uma agenda dedicada** chamada "Finanças" (nunca poluir a agenda principal do titular —
   e uma agenda dedicada pode ser compartilhada com cônjuge/contador sem expor o resto).
   Confirme a rota em `plan/ferramentas.md`; com `gog`, a mesma regra da planilha vale:
   `gog calendar --help` antes de qualquer comando. Se o escopo de Calendar não foi autorizado
   na F0, ofereça autorizar agora ou caia no fallback.
2. **Um evento por obrigação** de `consolidado/obrigacoes.csv`, com **alerta 7 dias antes + no
   dia**. Racional dos dois alertas: 7 dias é o prazo pra agir (transferir, contestar, parcelar);
   o do dia é a rede de segurança contra juros e multa.
3. **Evento-âncora "Rotina financeira mensal"**, recorrente no dia escolhido em `plan/perfil.md`,
   com o checklist da rotina na descrição. Na F5 use o checklist genérico de
   [templates/rotina-mensal.md](../templates/rotina-mensal.md); a F6 personaliza e re-publica a
   descrição (a re-publicação é idempotente, então isso é barato).
4. **Idempotência via `plan/agenda-map.csv`** — schema (contrato):
   `obrigacao_id,event_id,calendar_id,atualizado_em`. Antes de criar qualquer evento, procure o
   `obrigacao_id` no mapa: existe → atualize o evento (`event_id`); não existe → crie e registre
   a linha. **Re-publicar atualiza, nunca duplica.** Obrigação que saiu do consolidado tem seu
   evento removido e a linha retirada do mapa. Commite o mapa: ele é o estado da publicação.

**Sem Google:** os vencimentos de 30 dias já aparecem na Visão Geral do dashboard e a rotina
mensal (F6) revisa os 60 dias seguintes. Registre a limitação com o titular (sem alertas push) e
sugira um alarme recorrente no celular para o dia da rotina — baixa tecnologia, mas fecha o furo.

## Passo 4 — testar regenerabilidade e idempotência

Não pule: este teste é o gate da fase.

1. Re-publique a planilha sem mudar nada no repo → resultado idêntico (mesmos valores).
2. Re-publique a agenda 2× seguidas → contagem de eventos idêntica, zero duplicatas.
3. Apague uma aba (ou a planilha, se a rota permitir) e re-publique → tudo volta. Só o
   `agenda-map.csv` é insubstituível — por isso ele vive no git.

## Fechamento da fase

- [ ] `plan/consolidado/` gerado (obrigações 18m + fluxo de caixa 24m)
- [ ] Planilha com 6 abas publicada (ou dashboard.html no fallback); `plan/planilha-id.txt` salvo
- [ ] Agenda "Finanças" com alertas 7d+dia + evento-âncora da rotina — OU, no fallback,
      vencimentos na Visão Geral do dashboard e limitação registrada com o titular
- [ ] `plan/agenda-map.csv` commitado; teste de re-publicação passou sem duplicatas (apenas
      rota Google; não se aplica ao fallback)
- [ ] Titular abriu planilha e agenda no celular e confirmou que entende os espelhos
- [ ] Commit: `f5: planilha e agenda publicadas`

Próxima fase: [f6-rotina-e-governanca.md](f6-rotina-e-governanca.md) — instalar a operação.
