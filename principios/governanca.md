# Governança: quem decide o quê e como isso fica registrado

Este documento expande as doutrinas 5 a 7 do sistema (a versão operacional está em
[`../AGENTS.md`](../AGENTS.md)). A arquitetura de dados está em [`arquitetura.md`](arquitetura.md).
Este documento trata da arquitetura de **decisão**: o que o agente pode fazer sozinho, o que exige
o titular, e como o registro sobrevive entre sessões.

## 5. O agente propõe, o titular decide

**Regra:** toda decisão patrimonial é do titular. O agente analisa, recomenda e prepara. A
recomendação só vira ação depois de aceita, e a aceitação vira **registro** em
`plan/decisions.md`. O agente **nunca executa transação financeira**: não faz Pix, não compra
ativo, não resgata, não paga boleto. Prepara, agenda, lembra; quem move dinheiro é o titular.

**Racional:** três motivos independentes, cada um suficiente sozinho:

- **Responsabilidade.** Transferência executada por engano é irreversível. A fronteira "agente
  prepara, titular executa" põe o ato irreversível na mão de quem responde por ele.
- **Contexto.** O agente conhece o repo; o titular conhece fatos que não estão em arquivo nenhum.
  A recomendação certa nos dados pode ser errada por um desses fatos.
- **Auditoria.** Decisão registrada com racional pode ser reavaliada. Decisão executada em
  silêncio por um agente não pode ser explicada seis meses depois.

### O formato de decisão (contrato)

Cada entrada de `plan/decisions.md` (molde em
[`../templates/decisions.md`](../templates/decisions.md)) tem quatro campos, cada um com um
motivo:

| Campo | O que é | Por que existe |
|---|---|---|
| **Decisão** | o que foi decidido, numerado e datado | sem número não se cita; sem data não se contextualiza |
| **Racional** | por que, nas palavras de quem decidiu | é o que permite reavaliar quando o mundo mudar |
| **Dados no momento** | os números que embasaram, congelados | protege contra viés retrospectivo: a decisão é julgada pelo que se sabia, não pelo que se soube depois |
| **Pendências** | o que a decisão deixa em aberto | decisão que gera tarefa sem registro gera tarefa esquecida |

Exemplo (fictício): *"004: Reserva de emergência passa de 6 para 9 meses de custo fixo.
Racional: renda concentrada em um cliente. Dados no momento: custo fixo R$ 10.000/mês, reserva
atual R$ 60.000. Pendências: aportar R$ 30.000 até dez; revisar teto de renda fixa no comitê."*

## O mandato como briefing canônico

**Regra:** `plan/mandato.md` é o briefing canônico do patrimônio: objetivos, marcos, perfil de
risco, liquidez mínima, tetos e vetos, regras de alavancagem, IPS com bandas, política de aportes,
cadências. **Toda análise começa lendo o mandato.** Toda recomendação que o contrarie tem que
declarar o conflito; é proibido contornar o mandato em silêncio.

**Racional:** sem mandato, cada sessão de agente reconstrói os objetivos do titular do zero e
otimiza para o que imaginou, não para o que foi pedido. O mandato torna qualquer agente, em
qualquer sessão, capaz de dar a **mesma** resposta à pergunta "isso serve ao plano?". Ele nasce
provisório na F1 ([`../onboarding/f1-mandato.md`](../onboarding/f1-mandato.md)), é recalibrado com
o primeiro snapshot real (F4) e revisado anualmente. Mudá-lo é uma decisão registrada, como
qualquer outra.

## 6. Pendência estruturada, nunca silêncio

**Regra:** tudo que depende do titular vira pendência **registrada e estruturada**: a pergunta,
o **valor em jogo**, a evidência, e **o que muda conforme a resposta**. Nunca como anotação
informal do agente, nunca como mensagem avulsa no chat.

**Racional:** pendência sem valor em jogo não é priorizada. "Confirmar natureza de um Pix" não
recebe atenção do titular; "confirmar natureza de um Pix **de R$ 12.000** que hoje conta como
gasto e pode ser renda" recebe. O valor em jogo é o mecanismo de priorização. O "o que muda
conforme a resposta" permite ao titular decidir rápido, sem reconstruir o contexto.

Corolário (o mesmo de [`arquitetura.md`](arquitetura.md), visto do lado da governança):
**"rodou sem erro" não significa "nada a investigar"**. Guards imprimem AVISO e gravam pendência
para que a lacuna sobreviva ao fim da sessão.

### Registro operacional × vista de leitura

São dois arquivos porque são dois empregos. Misturá-los compromete os dois:

- **`plan/proximos-passos.md`** é o **registro operacional**: tabela `# | Ação | Data-limite |
  Status`, append-only, **nunca renumerar**. Itens são citados pelo número em outros arquivos e
  na saída de guards; renumerar quebra toda referência em silêncio. Data-limite e status vivem
  aqui; fechar um item é atualizar esta linha.
- **`plan/perguntas-abertas.md`** é a **vista de leitura** para o titular: as mesmas pendências,
  consolidadas e separadas por tipo (pergunta ao titular × ação do titular × decisão de método),
  cada uma com valor em jogo e evidência. É **regenerada** das fontes, cita
  `proximos-passos.md` pelo número e **nunca é o registro primário**: nenhuma pendência nasce
  nela.

**Racional da separação:** o registro precisa ser estável e citável (por isso append-only e
numerado); a vista precisa ser legível e reorganizável (por isso regenerada). Um arquivo único
perderia a estabilidade ou a legibilidade. Moldes em
[`../templates/proximos-passos.md`](../templates/proximos-passos.md) e
[`../templates/perguntas-abertas.md`](../templates/perguntas-abertas.md).

## 7. Uma pergunta por vez

**Regra:** nas entrevistas (F1, F3) e em qualquer coleta de informação, o agente faz **uma
pergunta por vez**, oferece múltipla escolha quando possível, e **registra a resposta antes de
fazer a próxima**. "Não sei / depois" é resposta válida: vira pendência em
`plan/proximos-passos.md` e a entrevista segue.

**Racional:** cinco perguntas em um bloco recebem respostas parciais: o titular responde o que é
fácil e o restante fica sem resposta. Múltipla escolha reduz o custo de responder (reconhecer é
mais barato que formular). Registrar antes de prosseguir torna a entrevista **retomável**: se a
sessão cair na pergunta 6, a próxima começa da 7, não do zero.

## Comitê trimestral

**Regra:** a cada trimestre, uma sessão de comitê com pauta fixa:

1. **Performance vs política.** A carteira fez o que o IPS previa?
2. **Desvios e rebalanceamento.** O que está fora da banda e o que fazer (decisão do titular;
   desvio de IPS vira pauta, **nunca rebalanceamento automático**).
3. **Riscos.** Revisão de `plan/riscos.md`: gatilhos disparados? Severidades mudaram?
4. **Análises do trimestre.** A fila de estudos encomendados (ex.: amortizar × investir). O
   protocolo de pesquisa e verificação de cada análise está em [`analises.md`](analises.md).
5. **Decisões.** Ata em `plan/decisions.md`, no formato padrão.

**Racional:** decisões de alocação tomadas dentro do mês reagem a ruído; agrupá-las em cadência
trimestral filtra o que se sustenta após 90 dias. A pauta é fixa para que o comitê responda
sempre às mesmas cinco perguntas, as que um family office responde ao titular. Entre comitês, o
sistema **opera** (rotina mensal), não **realoca**.

A **revisão anual do mandato** fecha o ciclo: uma vez por ano, os objetivos e as bandas voltam à
pauta, porque o plano também muda, em ritmo mais lento que a carteira. Protocolo de instalação
das cadências:
[`../onboarding/f6-rotina-e-governanca.md`](../onboarding/f6-rotina-e-governanca.md).
