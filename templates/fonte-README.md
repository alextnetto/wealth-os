<!-- instruções para o agente:
Template do README de um diretório de fonte (`fontes/<nome>/README.md`), criado na F2, um por
fonte declarada no inventário. Preencha "Como obter" com o passo a passo real (telas e menus
que o titular navega), não com instruções genéricas. Teste de qualidade: o titular consegue
exportar sozinho no mês seguinte lendo só este arquivo. "Como processar" fica pendente até o
primeiro raw real chegar. O script nasce do dado real: parser escrito contra formato suposto
quebra no primeiro arquivo recebido. Apague este comentário ao publicar.
-->

# Fonte: <nome; ex.: conta corrente Nubank>

## O que é

<uma frase: o que esta fonte cobre e o que fica de fora. Ex.: "conta corrente PF; o cartão de
crédito da mesma instituição é outra fonte, com fatura própria">

## Como obter o dado (passo a passo)

1. <ex.: app do banco → menu Extratos → Exportar → formato CSV>
2. <ex.: período: o mês fechado anterior (na primeira carga: 12 meses ou o que houver)>
3. Salvar em `raw/` com o período no nome: <ex.: `extrato-2026-07.csv` ou
   `extrato-2026-01..2026-06.csv`>

## Formato

- **Raw:** <ex.: CSV com colunas Data, Descrição, Valor; separador vírgula; encoding UTF-8>.
  Imutável: nunca editar. Erro no raw é tratado no processamento, com registro.
- **Consolidado:** <definido quando o 1º raw chegar; gerado por script, 100% reconstruível
  a partir do raw>

## Como processar

<comando/script, escrito pelo agente a partir do primeiro raw real; até lá: "pendente:
aguardando 1º raw (proximos-passos #<N>)">

## Cobertura

- De: <AAAA-MM> · Até: <AAAA-MM> · Lacunas: <meses faltantes, ou "nenhuma">

## Pendências

<itens desta fonte, citados pelo número do registro. Ex.: "#3 exportar 2026-01..2026-03".
Ou "nenhuma">
