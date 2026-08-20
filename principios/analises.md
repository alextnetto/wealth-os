# Análises: como o Conselheiro pesquisa, prova e recomenda

Este documento é o protocolo do modo **Conselheiro** (papéis e modos em
[`../AGENTS.md`](../AGENTS.md)). Toda análise nasce do mandato: abre citando o trecho de
`plan/mandato.md` que a governa e termina em decisão registrada em `plan/decisions.md`, no
formato de [`governanca.md`](governanca.md). Este documento define quando uma análise formal é
exigida, o fluxo de pesquisa aprofundada em 9 passos, a hierarquia de fontes e o formato de saída.

## Quando uma análise formal é exigida

**Regra:** o Conselheiro responde perguntas simples diretamente. Uma análise formal, pelo fluxo
completo abaixo, é obrigatória em dois casos:

1. **Valor em jogo relevante para o titular.** A resposta move um montante que importa na escala
   do patrimônio dele.
2. **Tema estrutural e difícil de reverter.** Exemplos: criar PJ, constituir holding, abrir
   offshore, mudar o regime de retirada, contratar alavancagem nova.

Toda análise nasce com registro na fila em `plan/proximos-passos.md`, com data. **Racional:** a
fila torna o estudo visível para o comitê e para qualquer agente em qualquer sessão; análise que
começa sem registro pode terminar sem decisão e sem dono.

## O fluxo de pesquisa aprofundada, em 9 passos

1. **Enquadramento.** O agente escreve a pergunta precisa e o valor em jogo em R$. Declara o que
   muda conforme a resposta: qual decisão depende dela e em que direção. Toda premissa entra
   explícita, com origem; premissa implícita impede a auditoria da análise.

2. **Pesquisa com prova.** Toda afirmação central exige fonte primária citada com data de
   acesso: lei, instrução normativa, texto oficial, tabela oficial de custos, contrato. Artigo
   de escritório e mídia servem de pista para achar a primária, nunca de prova. O relatório
   separa três camadas: o que o texto legal diz, qual é a interpretação dominante e o que é
   opinião do agente.

3. **Cenários e números.** O relatório compara 2 a 3 cenários com o custo total de cada um,
   incluindo o custo de manter a estrutura (contabilidade, taxas, obrigações acessórias). Cada
   cenário declara horizonte e ponto de equilíbrio: a partir de quando, e sob quais números, um
   cenário supera o outro.

4. **Verificação adversarial.** Uma verificação independente (outra sessão ou outro agente) tenta
   refutar cada afirmação central: refaz os números, confere cada citação contra a fonte e testa
   as premissas em cenário adverso. Divergência não resolvida vira pendência, nunca nota de
   rodapé. O resultado da verificação fica registrado no relatório.

5. **Recomendação com opções.** O relatório fecha com uma recomendação, as alternativas
   consideradas e o motivo de cada descarte. Declara também o que invalidaria a recomendação:
   quais fatos ou números, se mudarem, exigem refazer a conta.

6. **Validação profissional.** Em matéria tributária ou jurídica, contador ou advogado valida a
   análise antes de qualquer implementação. O agente prepara a pauta de perguntas e os números
   que sustentam cada uma. O agente não substitui o profissional.

7. **Decisão do titular.** O titular decide; a decisão entra em `plan/decisions.md`, com
   racional e os dados do momento. Recomendação sem decisão registrada não autoriza nenhum passo
   de implementação.

8. **Implementação.** Os passos de implementação entram em `plan/proximos-passos.md`, com
   prazos. Quem executa atos com efeito financeiro ou jurídico é o titular, conforme as regras
   de conduta de [`../AGENTS.md`](../AGENTS.md).

9. **Gatilho de revisão.** Análise tributária expira, porque legislação muda. Cada relatório
   declara o que dispara a re-análise: mudança de lei ou alíquota, mudança de faturamento ou de
   renda, ou uma data-limite. O gatilho vira item datado em `plan/proximos-passos.md`.

## Hierarquia de fontes

| Nível | Exemplos | Emprego na análise |
|---|---|---|
| **Fonte primária** | lei, instrução normativa, ato oficial, tabela oficial, contrato assinado | prova; é a única base válida para afirmação central |
| **Fonte secundária técnica** | parecer, artigo de escritório especializado | orienta a busca e aponta a primária; não prova |
| **Mídia e fórum** | notícia, post, discussão | não entra como prova em hipótese alguma |

## Estruturas tributárias: a classe padrão de análise

A maior parte das análises formais de um patrimônio brasileiro cai nesta classe. Temas e as
perguntas típicas de cada um:

- **Retirada PJ para PF.** Pró-labore × distribuição de lucros; encargos de cada via;
  efeito do fator R no enquadramento do Simples.
- **Holding imobiliária (PJ para deter imóveis).** ITBI na integralização; tributação do aluguel
  na PJ × na PF; ITCMD e planejamento sucessório; custos recorrentes de manutenção da PJ;
  ganho de capital na venda.
- **Offshore.** Regime brasileiro pós-Lei 14.754/2023 (fim do diferimento para controladas no
  exterior); custos de abertura e manutenção; sucessão; câmbio.
- **Previdência.** PGBL combinado com pró-labore; regime de tributação; prazos de resgate e de
  carência.
- **Mapa tributário por fonte de renda.** Carga atual de cada fonte de renda do titular e
  oportunidades legais de redução.

**Regra de doutrina:** leis e alíquotas citadas neste template são exemplos datados (2026). A
análise verifica a vigência de cada norma na data em que roda. Nenhuma estrutura é implementada
sem validação de contador ou advogado.

## Formato de saída

- **Relatório** em `docs/relatorios/AAAA-MM-DD-<tema>.md`, pelo molde
  [`../templates/analise.md`](../templates/analise.md).
- **Status do relatório**, um ciclo de quatro estados: rascunho (em elaboração), verificada
  (passou pela verificação adversarial do passo 4), decidida (a entrada existe em
  `plan/decisions.md`, passo 7), expirada (um gatilho de revisão do passo 9 disparou).
- **Decisão** em `plan/decisions.md`, no formato de [`governanca.md`](governanca.md).
- **Revisão agendada:** o gatilho de revisão (passo 9) registrado como item datado em
  `plan/proximos-passos.md` e, quando couber, como pauta do próximo comitê.
