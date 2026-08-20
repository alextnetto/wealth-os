# Arquitetura: como o dado vive neste repo

Este documento expande as doutrinas 1 a 4 do sistema (a versão operacional, em forma de regras para
o agente, está em [`../AGENTS.md`](../AGENTS.md)). Cada regra vem com o racional. Quem discordar do
racional deve mudar a regra com uma decisão registrada, nunca ignorá-la em silêncio.

## 1. O repo é a fonte da verdade

**Regra:** todo dado, decisão e configuração do sistema vive neste repositório git, privado e local.
Google Sheets e Google Calendar são **espelhos publicados**: regeneráveis do zero, em mão única
(repo → espelho), nunca editados diretamente. Nada é digitado duas vezes.

**Racional:** um sistema com duas fontes da verdade não tem nenhuma. Planilhas editadas à mão
divergem do registro em semanas. Depois disso, ninguém sabe qual número vale. Com o repo como fonte
única:

- Apagar a planilha e republicar produz o mesmo resultado. O espelho não carrega estado.
- Uma célula editada direto no Sheets é **perdida na próxima publicação, por design**. Se o número
  está errado, o erro está no repo (ou na fonte); corrija lá e republique.
- Git fornece o que planilha não fornece: histórico auditável, diff de cada mudança, blame,
  rollback. A pergunta "por que o patrimônio caiu em março?" tem resposta em `git log`.

O mesmo vale para a agenda: os eventos de vencimento são gerados de `plan/recorrentes.csv`; a
idempotência via `plan/agenda-map.csv` (obrigação → evento) garante que republicar **atualiza**,
nunca duplica. Detalhes de publicação:
[`../onboarding/f5-planilha-e-agenda.md`](../onboarding/f5-planilha-e-agenda.md).

## 2. Raw imutável → derivado determinístico

**Regra:** todo dado bruto (extrato, fatura, export) entra em `fontes/<fonte>/raw/` e **nunca é
editado**: nem para corrigir um typo, nem para remover uma linha aparentemente inválida. Toda
camada consolidada é gerada por script determinístico e 100% reconstruível: apagar
`plan/consolidado/` e rodar de novo produz saída idêntica.

**Racional:** o raw é a evidência. Um CSV de extrato corrigido à mão retira do repo o valor de
prova: deixa de ser possível distinguir o dado do banco da opinião de quem editou. Correções
acontecem em uma de duas camadas, nunca no raw:

- **Erro de export:** re-exportar da fonte e substituir o arquivo inteiro (commit dizendo por quê).
- **Erro de leitura ou classificação:** regra nas camadas de classificação
  (`plan/regras-classificacao.csv`, `plan/excecoes.csv`), versionada e auditável.

O **nome do raw codifica a cobertura temporal do conteúdo** (ex.: `extrato-2026-01..2026-06.csv`).
Racional: a lacuna de dados mais perigosa é a invisível. Com a cobertura no nome, listar o
diretório mostra o que falta.

## 3. Scripts nascem do dado real, não do imaginado

**Regra:** o template não traz nenhum script pronto. O agente escreve o consolidador quando o
primeiro extrato real chegar (F2/F3), contra aquele arquivo. Todo script obedece o contrato:
**determinístico, idempotente, falha ruidosa, com teste**.

**Racional:** parser escrito contra dado imaginado quebra no primeiro arquivo real. Cada banco tem
seu encoding, seu separador, sua forma de escrever estorno. Escrever contra o dado real inverte o
risco: o script é validado desde o início contra o caso que importa. O contrato, item a item:

| Propriedade | O que significa | Por quê |
|---|---|---|
| Determinístico | mesma entrada → mesma saída, sempre | é o que torna o derivado reconstruível (doutrina 2) |
| Idempotente | rodar 2× = rodar 1× | a rotina mensal pode rodar de novo sem risco; falha no meio não corrompe |
| Falha ruidosa | dado inválido → erro fatal com mensagem | falha silenciosa produz número errado com aparência de correto |
| Com teste | ao menos os casos que já quebraram | regressão silenciosa em dinheiro é inaceitável |

**Duas famílias de problema, dois comportamentos.** A distinção é doutrina, não estilo:

- **Erro fatal de dado** (`die`): categoria fora da taxonomia, id duplicado, câmbio faltando.
  O script **para**. Racional: seguir adiante propagaria dado inválido para todos os consolidados.
  O erro exige correção no dado antes de qualquer nova execução; repetir a execução sem corrigir é
  proibido.
- **Guard de atenção** (`AVISO` + pendência estruturada): o dado é válido, mas algo exige decisão
  do titular. Exemplos: um gasto sem par no ledger, um mês com desvio acima da banda. O script
  termina, imprime o aviso com o **valor em jogo** e grava a pendência (formato em
  [`governanca.md`](governanca.md)). Racional: guard que decide sozinho toma uma decisão que
  pertence ao titular. Aviso que existe só no stdout se perde ao fim da sessão; por isso o guard
  grava a pendência.

Corolário obrigatório: **"rodou sem erro" não significa "nada a investigar"**. Quem executa a
rotina lê o stdout completo, não só o exit code.

## 4. Append-only nas séries e taxonomias

**Regra:** taxonomias (`plan/categorias.csv`, regras, exceções) só crescem pelo fim: categoria
nova entra na última linha; é proibido reordenar e remover. Séries históricas fechadas (meses
consolidados, snapshots) nunca se editam. **Restatement é decisão explícita**: commit próprio,
dizendo o que mudou, por quê, e quais meses foram reabertos.

**Racional:** a ordem das linhas é contrato. Itens são citados por número em outros arquivos e em
pendências; renumerar quebra toda referência existente em silêncio. Editar histórico fechado
reclassifica o passado sem aviso em nenhum relatório antigo: o gasto de março muda de valor meses
depois e ninguém sabe por quê. O restatement com commit próprio preserva as duas coisas que
importam: o número novo **e** a prova de que ele mudou.

## Convenção de fontes

**Regra:** cada fonte de dados (banco, cartão, corretora, exchange) é um diretório
`fontes/<nome>/` com duas coisas obrigatórias:

1. `README.md`, pelo template [`../templates/fonte-README.md`](../templates/fonte-README.md):
   **o que é** o dado, **como obter** (passo a passo clicável, do login ao download), **formato**
   (raw e consolidado), **como processar**, **cobertura** (de/até) e pendências.
2. `raw/`: os arquivos brutos, imutáveis, nomeados pela cobertura (doutrina 2).

**Racional:** seis meses depois, ninguém lembra como se exporta o extrato da corretora: nem o
titular, nem o agente da próxima sessão. O README da fonte é o runbook: qualquer agente, em
qualquer sessão, reconstrói o processo lendo um arquivo. A cobertura declarada no README permite
ao sistema distinguir "gastou zero" de "não temos o dado". Confundir os dois é o erro mais caro
que um sistema financeiro pode cometer.

O protocolo de criação das fontes é a F2:
[`../onboarding/f2-fontes-e-extratos.md`](../onboarding/f2-fontes-e-extratos.md).

## Espelhos: planilha e agenda

**Regra:** os espelhos publicados são dois: a **planilha mestra** (6 abas: Visão Geral,
Obrigações, Fluxo de Caixa, Patrimônio, Orçado × Realizado, Riscos) e a **agenda "Finanças"**
(um evento por obrigação, alerta 7 dias antes + no dia, mais o evento-âncora da rotina mensal).
Sem Google, os mesmos dados saem em `plan/consolidado/*.csv` + dashboard HTML local. Espelho é
formato, não dependência.

**Racional:** o repo serve à operação; não serve à consulta rápida do titular no celular. O
espelho existe para o titular consultar; o repo, para o sistema registrar e operar. A publicação
regenerável e de mão única permite ter os dois sem criar uma segunda fonte da verdade, o problema
que a doutrina 1 impede.
