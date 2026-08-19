# Arquitetura — como o dado vive neste repo

Este documento expande as doutrinas 1–4 do sistema (a versão operacional, em forma de regras para
o agente, está em [`../AGENTS.md`](../AGENTS.md)). Cada regra vem com o racional — se você discordar
do racional, mude a regra com uma decisão registrada; nunca a ignore em silêncio.

## 1. O repo é a fonte da verdade

**Regra:** todo dado, decisão e configuração do sistema vive neste repositório git, privado e local.
Google Sheets e Google Calendar são **espelhos publicados**: regeneráveis do zero, em mão única
(repo → espelho), nunca editados diretamente. Nada é digitado duas vezes.

**Racional:** um sistema com duas fontes da verdade não tem nenhuma. Planilhas editadas à mão
divergem do registro em semanas e ninguém sabe mais qual número vale. Com o repo como fonte única:

- Apagar a planilha inteira e republicar produz o mesmo resultado — o espelho não carrega estado.
- Uma célula editada direto no Sheets é **perdida na próxima publicação, por design**. Se o número
  está errado, o erro está no repo (ou na fonte); corrija lá e republique.
- Git dá de graça o que planilha nenhuma dá: histórico auditável, diff de cada mudança, blame,
  rollback. "Por que o patrimônio caiu em março?" tem resposta em `git log`.

O mesmo vale para a agenda: eventos de vencimento são gerados de `plan/recorrentes.csv`; a
idempotência via `plan/agenda-map.csv` (obrigação → evento) garante que republicar **atualiza**,
nunca duplica. Detalhes de publicação:
[`../onboarding/f5-planilha-e-agenda.md`](../onboarding/f5-planilha-e-agenda.md).

## 2. Raw imutável → derivado determinístico

**Regra:** todo dado bruto (extrato, fatura, export) entra em `fontes/<fonte>/raw/` e **nunca é
editado** — nem para corrigir um typo, nem para remover uma linha "óbvia". Toda camada consolidada
é gerada por script determinístico e 100% reconstruível: apagar `plan/consolidado/` e rodar de
novo produz saída idêntica.

**Racional:** o raw é a evidência. No momento em que alguém "arruma" um CSV de extrato à mão, o
repo deixa de provar qualquer coisa — não dá mais para distinguir dado do banco de opinião de quem
editou. Correções acontecem em uma de duas camadas, nunca no raw:

- **Erro de export** → re-exportar da fonte e substituir o arquivo inteiro (commit dizendo por quê).
- **Erro de leitura/classificação** → regra nas camadas de classificação
  (`plan/regras-classificacao.csv`, `plan/excecoes.csv`), versionada e auditável.

O **nome do raw codifica a cobertura temporal do conteúdo** (ex.: `extrato-2026-01..2026-06.csv`).
Racional: a lacuna de dados mais perigosa é a invisível — com a cobertura no nome, "o que falta"
é uma olhada no diretório, não uma investigação.

## 3. Scripts nascem do dado real, não do imaginado

**Regra:** o template não traz nenhum script pronto. O agente escreve o consolidador quando o
primeiro extrato real chegar (F2/F3), contra aquele arquivo — e todo script obedece o contrato:
**determinístico, idempotente, falha ruidosa, com teste**.

**Racional:** parser escrito contra dado imaginado quebra no primeiro arquivo de verdade — cada
banco tem seu encoding, seu separador, sua forma de escrever estorno. Escrever contra o dado real
inverte o risco: o script já nasce provado contra o caso que importa. O contrato, item a item:

| Propriedade | O que significa | Por quê |
|---|---|---|
| Determinístico | mesma entrada → mesma saída, sempre | é o que torna o derivado reconstruível (doutrina 2) |
| Idempotente | rodar 2× = rodar 1× | rotina mensal reroda sem medo; falha no meio não corrompe |
| Falha ruidosa | dado inválido → erro fatal com mensagem | falhar calado produz número errado com cara de certo |
| Com teste | ao menos os casos que já quebraram | regressão silenciosa em dinheiro é inaceitável |

**Duas famílias de problema, dois comportamentos** — e a distinção é doutrina, não estilo:

- **Erro fatal de dado** (`die`): categoria fora da taxonomia, id duplicado, câmbio faltando.
  O script **para**. Racional: seguir adiante propagaria lixo para todos os consolidados; o
  erro exige correção no dado antes de qualquer rerun — nunca "rodar de novo e ver se passa".
- **Guard de atenção** (`AVISO` + pendência estruturada): o dado é válido, mas algo pede decisão
  do titular — um gasto sem par no ledger, um mês com desvio acima da banda. O script termina,
  imprime o aviso com o **valor em jogo** e grava a pendência (formato em
  [`governanca.md`](governanca.md)). Racional: guard que decide sozinho está usurpando decisão
  do titular; guard que só imprime no stdout é esquecido — por isso a pendência é gravada.

Corolário obrigatório: **"rodou sem erro" ≠ "nada a investigar"**. Quem executa a rotina lê o
stdout inteiro, não só o exit code.

## 4. Append-only nas séries e taxonomias

**Regra:** taxonomias (`plan/categorias.csv`, regras, exceções) só crescem pelo fim — categoria
nova entra na última linha, nunca se reordena nem se remove. Séries históricas fechadas (meses
consolidados, snapshots) nunca se editam. **Restatement é decisão explícita**: commit próprio,
dizendo o que mudou, por quê, e quais meses foram reabertos.

**Racional:** a ordem das linhas é contrato — itens são citados por número em outros arquivos e em
pendências; renumerar quebra toda referência existente em silêncio. E editar histórico fechado
reclassifica o passado sem que nenhum relatório antigo avise: o "gasto de março" muda de valor
meses depois e ninguém sabe por quê. O restatement com commit próprio preserva as duas coisas que
importam: o número novo **e** a prova de que ele mudou.

## Convenção de fontes

**Regra:** cada fonte de dados (banco, cartão, corretora, exchange…) é um diretório
`fontes/<nome>/` com duas coisas obrigatórias:

1. `README.md` — pelo template [`../templates/fonte-README.md`](../templates/fonte-README.md):
   **o que é** o dado, **como obter** (passo a passo clicável, do login ao download), **formato**
   (raw e consolidado), **como processar**, **cobertura** (de/até) e pendências.
2. `raw/` — os arquivos brutos, imutáveis, nomeados pela cobertura (doutrina 2).

**Racional:** seis meses depois, ninguém lembra como se exporta o extrato da corretora — nem o
titular, nem o agente da próxima sessão. O README da fonte é o runbook: qualquer agente, em
qualquer sessão, reconstrói o processo do zero lendo um arquivo. A cobertura declarada no README
é o que permite ao sistema distinguir "gastou zero" de "não temos o dado" — confundir os dois é
o erro mais caro que um sistema financeiro pode cometer.

O protocolo de criação das fontes é a F2:
[`../onboarding/f2-fontes-e-extratos.md`](../onboarding/f2-fontes-e-extratos.md).

## Espelhos — planilha e agenda

**Regra:** os espelhos publicados são dois — a **planilha mestra** (6 abas: Visão Geral,
Obrigações, Fluxo de Caixa, Patrimônio, Orçado × Realizado, Riscos) e a **agenda "Finanças"**
(um evento por obrigação, alerta 7 dias antes + no dia, mais o evento-âncora da rotina mensal).
Sem Google, os mesmos dados saem em `plan/consolidado/*.csv` + dashboard HTML local — espelho é
formato, não dependência.

**Racional:** o repo é ótimo para operar e péssimo para consultar no celular na fila do mercado.
O espelho existe para o titular **ver**; o repo, para o sistema **saber**. Manter a publicação
regenerável e de mão única é o que permite ter os dois sem criar uma segunda fonte da verdade —
que é exatamente o problema que a doutrina 1 existe para impedir.
