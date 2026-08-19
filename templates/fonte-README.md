<!-- instruções pro agente:
Template do README de um diretório de fonte (`fontes/<nome>/README.md`), criado na F2 — um por
fonte declarada no inventário. Preencha "Como obter" com o passo a passo REAL (telas e menus
que o titular navega), não instruções genéricas: o teste de qualidade é o titular conseguir
exportar sozinho no mês que vem lendo só isto. "Como processar" fica pendente até o primeiro
raw real chegar — script nasce do dado real, não do imaginado (parser contra dado imaginado
quebra no primeiro arquivo de verdade). Apague este comentário ao publicar.
-->

# Fonte: <nome — ex.: conta corrente Nubank>

## O que é

<uma frase: o que esta fonte cobre e o que fica de fora — ex.: "conta corrente PF; o cartão de
crédito da mesma instituição é outra fonte, com fatura própria">

## Como obter o dado (passo a passo)

1. <ex.: app do banco → menu Extratos → Exportar → formato CSV>
2. <ex.: período: o mês fechado anterior (na primeira carga: 12 meses ou o que houver)>
3. Salvar em `raw/` com o período no nome: <ex.: `extrato-2026-07.csv` ou
   `extrato-2026-01..2026-06.csv`>

## Formato

- **Raw:** <ex.: CSV com colunas Data, Descrição, Valor; separador vírgula; encoding UTF-8>.
  Imutável — nunca editar; erro no raw se trata no processamento, com registro.
- **Consolidado:** <definido quando o 1º raw chegar — gerado por script, 100% reconstruível
  a partir do raw>

## Como processar

<comando/script — escrito pelo agente com o primeiro raw real em mãos; até lá: "pendente —
aguardando 1º raw (proximos-passos #<N>)">

## Cobertura

- De: <AAAA-MM> · Até: <AAAA-MM> · Buracos: <meses faltantes, ou "nenhum">

## Pendências

<itens desta fonte citados por número do registro — ex.: "#3 exportar 2026-01..2026-03";
ou "nenhuma">
