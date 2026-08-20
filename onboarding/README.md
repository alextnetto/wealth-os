# Onboarding: protocolo

Este diretório transforma o template vazio no sistema financeiro operante do titular. O agente
executa; papel, modos e regras de conduta estão em [AGENTS.md](../AGENTS.md), que vence em
qualquer conflito. O titular decide, sempre.

São sete fases, F0-F6, cada uma com arquivo próprio, gate de saída verificável e valor entregue
mesmo que o onboarding pare ali. **Cada fase entrega valor sozinha e destrava a seguinte.** Isso
é decisão de desenho: quem para na F3 sai com mandato, fontes organizadas e orçamento, e nada do
que foi feito depende do que ainda não foi.

## As fases

| Fase | Entrega | O que depende do titular | ~Tempo |
|---|---|---|---|
| [F0: preparação](f0-preparacao.md) | repo privado confirmado, ferramentas detectadas, esqueleto de `plan/` | nome, moeda-base, dia da rotina | 15 min |
| [F1: mandato](f1-mandato.md) | `plan/mandato.md` v1: número-alvo, risco, tetos, IPS, cadências | entrevista de 10 blocos | 45 a 60 min |
| [F2: fontes e extratos](f2-fontes-e-extratos.md) | `fontes/<fonte>/` com README + `raw/` para cada fonte declarada | exportar extratos (12 meses ou o que houver) | 30 a 60 min + espera dos exports |
| [F3: orçamento e recorrentes](f3-orcamento-e-recorrentes.md) | `plan/orcamento.csv`, `recorrentes.csv`, `categorias.csv` + 1º fluxo de caixa | entrevista de renda e fixos; validar metas | 45 a 60 min |
| [F4: patrimônio](f4-patrimonio.md) | `plan/patrimonio.csv` (snapshot datado) + patrimônio líquido | saldos que o raw não cobre | 30 a 45 min |
| [F5: planilha e agenda](f5-planilha-e-agenda.md) | planilha mestra (6 abas) + agenda "Finanças" com alertas, ou dashboard local | autorizar Google, ou aceitar o fallback | 30 min |
| [F6: rotina e governança](f6-rotina-e-governanca.md) | `plan/rotina-mensal.md`, `plan/riscos.md`, 1º comitê agendado | escolher datas; validar riscos | 30 a 45 min |

Total típico: 4 a 6 horas de conversa, em 2 a 5 sessões (ver "Sessões" abaixo).

## Gates: quando uma fase fecha

Uma fase só fecha quando **(a)** os artefatos do gate existem no repo (checklist de saída no
arquivo da fase) e **(b)** o titular confirma que o conteúdo reflete a realidade dele. Racional:
gate por artefato é verificável por qualquer agente em qualquer sessão; a confirmação impede
avançar sobre entendimento errado. Corrigir o mandato na F1 custa uma frase; na F6, custa
retrabalho em cadeia.

- **Pendência registrada conta como gate cumprido** quando o arquivo da fase assim determinar
  (ex.: a F2 fecha com fonte que tem dir + README + pendência datada, mesmo sem raw). O que não
  pode existir é lacuna sem registro: ou o artefato existe, ou a pendência está em
  `plan/proximos-passos.md`.
- Fase fechada = commit com mensagem da fase (ex.: `f2: fontes e extratos`). O histórico do git
  é a trilha de auditoria do onboarding.
- Não pule fase nem rode duas em paralelo: cada fase consome artefatos da anterior (o orçamento
  da F3 classifica extratos da F2; o snapshot da F4 recalibra o IPS provisório da F1).

## Como conduzir

Valem todas as regras de conduta de [AGENTS.md](../AGENTS.md). Na entrevista, as mais relevantes:

- **Uma pergunta por vez.** Múltipla escolha quando possível: reduz atrito e produz resposta
  comparável. Registre a resposta no arquivo de destino **antes** de fazer a próxima pergunta.
- **"Não sei" e "depois" são respostas válidas.** Viram pendência com data e valor em jogo em
  `plan/proximos-passos.md`, e a entrevista segue. Nada trava o onboarding à espera de resposta.
- **Nunca invente dado.** Célula sem fonte é célula vazia + pendência. Rascunho proposto por
  você (ex.: IPS conservador na F1) entra sempre **marcado** como provisório.
- **Explique o porquê de cada bloco antes de perguntar.** O titular responde melhor sabendo para
  que serve a resposta. Os arquivos de fase trazem o racional de cada pergunta.
- **Anuncie o progresso dentro da fase.** "Bloco 4 de 10 do mandato" ocupa uma linha e informa
  ao titular quanto falta.

## Retomada

O onboarding é retomável por construção: qualquer sessão nova detecta a fase pelo estado do repo
e continua do ponto em que parou. Resumo da detecção: **o primeiro artefato ausente define a
fase**.

| Artefato ausente | Fase |
|---|---|
| `plan/` (ou `plan/perfil.md` / `plan/ferramentas.md`) | F0 |
| `plan/mandato.md` | F1 |
| fonte completa em `fontes/` (dir + README + `raw/` ou pendência) | F2 |
| `plan/orcamento.csv` + `recorrentes.csv` + `categorias.csv` | F3 |
| `plan/patrimonio.csv` | F4 |
| espelho (`plan/planilha-id.txt` ou `plan/consolidado/dashboard.html`) | F5 |
| `plan/rotina-mensal.md` + `plan/riscos.md` | F6 |
| nenhum (tudo existe) | operação |

A tabela canônica, com sinais completos, modos e regras de uso, está em
[AGENTS.md](../AGENTS.md). Se as duas divergirem, a de lá vence. Ao retomar:

1. Rode a detecção e o checklist de saída da fase (a fase pode estar pela metade).
2. Anuncie: "estamos na F3; já existem X e Y; falta Z".
3. Releia o que já está registrado na fase. **Nunca** repita pergunta que já tem resposta
   gravada: repetir sinaliza ao titular que o registro não tem valor.

## Sessões

O onboarding completo leva **2 a 5 sessões**. A pausa natural é o fim de fase: gate fechado,
commit feito, valor entregue. O titular sai com algo pronto mesmo se demorar a voltar.

- Sugira a pausa ao fechar um gate ("bom ponto para parar; na próxima sessão começamos a F4").
  Sessão longa demais degrada a qualidade das respostas de entrevista.
- A F1 costuma exigir uma sessão inteira: é a conversa mais importante do sistema.
- A F2 tem espera embutida (o titular precisa exportar extratos dos bancos). Feche a F2 com as
  pendências registradas e siga: a entrevista da F3 não depende dos extratos. A primeira
  classificação automática roda quando o raw chegar.
- Ao encerrar **qualquer** sessão: commit do que está pronto, resumo do que ficou pendente e o
  primeiro passo da próxima sessão em uma frase (com prazo em `plan/proximos-passos.md`, se
  couber prazo).
