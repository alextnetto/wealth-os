<!-- instruções para o agente:
Molde do relatório de análise do Conselheiro. Não preencha este arquivo em templates/: copie
para `docs/relatorios/AAAA-MM-DD-<tema>.md` no repo do titular (ex.:
`docs/relatorios/2026-03-10-amortizar-ou-investir.md`) e preencha lá. Apague este comentário
no arquivo final. Regras:
- Toda análise começa lendo `plan/mandato.md`. Cite no cabeçalho o trecho que a governa.
- Nunca invente dado. Afirmação sem fonte primária não entra na tabela de evidências; vira
  pendência em `plan/proximos-passos.md`, com valor em jogo.
- Status: rascunho (em elaboração), verificada (passou pela verificação adversarial), decidida
  (o titular decidiu e a entrada existe em `plan/decisions.md`), expirada (um gatilho de
  revisão disparou; a análise não fundamenta mais decisão).
- Análise em rascunho não sobe para verificada sem verificação adversarial: outra sessão ou
  outro agente refaz os números críticos antes de o titular decidir.
- A pauta para contador ou advogado é obrigatória em matéria tributária ou jurídica. Nos
  demais temas, mantenha a seção e escreva "não se aplica", com o motivo.
- Os links relativos abaixo assumem o arquivo final em `docs/relatorios/`.
- As linhas marcadas "EXEMPLO FICTÍCIO" mostram o nível de detalhe esperado: apague-as.
-->

# Análise: <tema>

- **Tema:** <tema em poucas palavras>
- **Data:** <AAAA-MM-DD>
- **Status:** <rascunho | verificada | decidida | expirada>
- **Protocolo:** principios/analises.md
- **Trecho do mandato que governa esta análise:** "<citação de plan/mandato.md>"

## Pergunta e valor em jogo

**Pergunta (uma frase):** <a decisão que esta análise fundamenta, formulada como pergunta>

**Valor em jogo:** <R$ estimado> · <o que muda conforme a resposta>

> EXEMPLO FICTÍCIO: "Amortizar o financiamento ou investir o excedente mensal?" Valor em jogo:
> R$ 18.000 em 24 meses; a resposta define o destino de R$ 2.500 por mês.

## Premissas

Cada premissa declara a origem: mandato, resposta do titular, fonte em `fontes/`, ou
estimativa marcada com método e data.

- <premissa> (origem: <mandato | titular em AAAA-MM-DD | fontes/<fonte> | estimativa: método e data>)
- <ex.: o excedente mensal de R$ 2.500 se mantém por 24 meses> (origem: <ex.: plan/orcamento.csv, fluxo de caixa de AAAA-MM>)

## Evidências

Só entra afirmação com fonte primária: contrato, lei, extrato em `fontes/<fonte>/raw/`,
documento oficial. A coluna Verificação diz quem conferiu e como.

| Afirmação | Fonte primária | Data de acesso | Verificação |
|---|---|---|---|
| <afirmação verificável> | <documento, lei, contrato, extrato> | <AAAA-MM-DD> | <quem conferiu e como, ou "pendente"> |
| <ex.: o CET do financiamento é 11,2% a.a.> | <ex.: contrato em fontes/banco-x/raw/> | <AAAA-MM-DD> | <ex.: recalculado nesta sessão a partir do fluxo do contrato> |

## Cenários

2 a 3 cenários. Cada um com custo total no horizonte e ponto de equilíbrio: a condição que
inverte a comparação entre eles.

| Cenário | Custo total | Horizonte | Ponto de equilíbrio |
|---|---|---|---|
| A: <descrição> | <R$> | <ex.: 24 meses> | <condição que o torna pior que B> |
| B: <descrição> | <R$> | <horizonte> | <condição> |
| C (opcional): <descrição> | <R$> | <horizonte> | <condição> |

## Recomendação e opções

**Recomendação:** <cenário recomendado e por quê, em 2 a 4 frases, ancoradas no mandato e nos
números acima>

**Opções não recomendadas:** <por que cada uma perde, em uma frase cada>

**O que invalidaria a recomendação:** <fatos ou números que, se mudarem, invertem a conclusão>

> EXEMPLO FICTÍCIO: invalidaria a recomendação uma queda do CET abaixo de 9% a.a. ou a perda
> da renda que sustenta o excedente mensal.

## Pauta para contador ou advogado

Obrigatória em matéria tributária ou jurídica. Perguntas numeradas e específicas, com o
contexto mínimo para o profissional responder. Nos demais temas: "não se aplica" e o motivo.

1. <pergunta>
2. <pergunta>

> EXEMPLO FICTÍCIO: "1. A isenção de ganho de capital na venda de imóvel residencial (art. 39
> da Lei 11.196/2005) alcança o uso do valor para amortizar financiamento de outro imóvel no
> prazo de 180 dias?"

## Verificação adversarial

Outra sessão ou outro agente refaz os números críticos antes de a análise subir de rascunho.
A verificação não relê a prosa: reabre as fontes e recalcula.

- **Quem verificou:** <sessão ou agente>, em <AAAA-MM-DD>
- **O que foi refeito:** <cálculos recalculados e fontes reabertas>
- **Resultado:** <confirmado | divergência: qual, e o que mudou na análise>

## Decisão

<link para a entrada em [../../plan/decisions.md](../../plan/decisions.md), com o número da
entrada | pendente>

## Gatilho de revisão

O que dispara a re-análise e muda o status para expirada. Sem gatilho, a análise envelhece sem
aviso e continua citada como se valesse.

- <ex.: mudança na lei ou na taxa que ancora a tabela de evidências>
- <ex.: evento: venda do imóvel, troca de emprego, fim do contrato>
- <ex.: data: rever em AAAA-MM-DD se nada disparar antes>
