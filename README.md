# wealth-os

> **seu family office, operado pelo seu agente**

**wealth-os** is a public, whitelabel GitHub template for running your entire financial life out
of a private git repo, operated by the AI agent you already use (Claude Code, Codex, Cursor…).
Copy the template into a private repo, open your agent in the folder, say *"faz meu onboarding"* —
the agent interviews you, builds your investment mandate, consolidates statements, publishes a
master spreadsheet and a due-date calendar, and installs a monthly financial routine. Everything
below is in Brazilian Portuguese; the architecture itself is country-agnostic.

---

## O pitch

Sua vida financeira hoje mora em lugares ruins:

- **Planilha morta.** Você montou uma vez, alimentou três meses e abandonou — porque digitar dado
  na mão toda semana não é um sistema, é uma penitência.
- **Apps de banco.** Um silo por instituição, cada um mostrando só o pedaço que lhe interessa
  vender. Ninguém responde "quanto sobra por mês?" nem "estou andando na direção da minha meta?".
- **Consultor / family office.** Resolve de verdade — cobrando caro e, em geral, a partir de um
  patrimônio que a maioria das pessoas ainda não tem.

Um repo git + um agente de IA resolve melhor, por três razões:

1. **O trabalho chato fica com o agente.** Classificar fatura, consolidar extrato, projetar fluxo
   de caixa, republicar planilha e agenda — tudo isso é exatamente o tipo de trabalho que um
   agente faz bem e você odeia fazer. Você só exporta os extratos e toma as decisões.
2. **O repo é a fonte da verdade; planilha e agenda são espelhos.** Tudo versionado em git, no SEU
   repo privado: dá para auditar qualquer número até o extrato de origem, e dá para apagar a
   planilha inteira e regenerá-la idêntica. Nada é digitado duas vezes.
3. **Governança de gente grande, custo de token.** Mandato escrito (IPS), decisões registradas com
   racional, rotina mensal de 30–45 min, comitê trimestral. É o protocolo de um family office —
   sem o family office.

O template não traz nenhum dado: só protocolo, templates e princípios. Seus dados nascem no seu
repo privado e ficam lá.

## Quickstart — 4 passos

1. **Crie seu repo PRIVADO.** Clique em **"Use this template"** no GitHub e marque **Private**
   (ou clone e suba para um repo privado seu). Privado não é opcional: este repo vai conter sua
   vida financeira inteira.
2. **Traga o repo para a sua máquina.** No seu repo privado, clique em **Code** → copie o
   endereço e rode `git clone <endereço>` no terminal — ou use o GitHub Desktop
   (Code → Open with GitHub Desktop) se preferir não usar terminal. Entre na pasta criada.
3. **Abra seu agente na pasta.** `claude`, `codex`, ou qualquer agente que leia arquivos e rode
   shell.
4. **Diga: "faz meu onboarding".** O agente assume dali — entrevista, extratos, orçamento,
   patrimônio, planilha, agenda, rotina. Leva de 2 a 5 sessões e é retomável: pare quando quiser,
   o agente detecta onde parou e continua.

## O que você ganha ao final

Cada fase entrega valor sozinha e destrava a seguinte — você não precisa terminar tudo para já
estar melhor do que antes.

| Fase | O que acontece | O que você leva |
|------|----------------|-----------------|
| **F0 — Preparação** | checagem de privacidade, detecção de ferramentas, esqueleto do repo | repo seguro e pronto para operar |
| **F1 — Mandato** | entrevista: sua meta, risco, liquidez, tetos, alocação-alvo | `plan/mandato.md` — seu IPS escrito |
| **F2 — Fontes e extratos** | inventário de contas, cartões, corretora, cripto, imóveis, dívidas | toda fonte documentada + 12 meses de dados brutos |
| **F3 — Orçamento** | renda, fixos, obrigações datadas, taxonomia e classificação de gastos | orçamento real + primeiro fluxo de caixa projetado |
| **F4 — Patrimônio** | snapshot de balanço: ativos, passivos, PF e PJ | patrimônio líquido datado, comparado à alocação-alvo |
| **F5 — Planilha e agenda** | publicação dos espelhos | planilha mestra (6 abas) + agenda com alertas de vencimento |
| **F6 — Rotina e governança** | instalação das cadências e registro de riscos | rotina mensal de 30–45 min + comitê trimestral agendado |

## Requisitos

- **Um agente de IA com acesso a arquivos e shell.** Claude Code, Codex CLI, Cursor, ou
  equivalente. O template é agnóstico: as instruções vivem em [AGENTS.md](AGENTS.md), que
  virou convenção entre os principais agentes.
- **Opcional — Google Sheets e Calendar**, via CLI `gog` ou MCP, para a planilha mestra e a
  agenda de vencimentos com alertas.
- **Sem Google funciona igual:** o agente gera CSVs consolidados + um dashboard HTML local com
  as mesmas visões. A planilha é espelho, não fonte — por isso é dispensável.

## Privacidade

- **Seus dados nunca saem do SEU repo privado.** O template é público; a sua cópia é privada e
  desconectada dele. Não existe backend, telemetria, coleta — nada volta para cá.
- O onboarding começa (F0) confirmando que o repo é privado e que `.gitignore` cobre `.env` e
  chaves. O agente checa `git remote -v` antes de qualquer push.
- **O agente nunca pede senha de banco.** Extratos são exportados por você, no site do banco, e
  colocados no repo. Cripto entra só por endereço público.
- Doutrina completa e checklist: [principios/privacidade.md](principios/privacidade.md).

## Como funciona por dentro

O repo é a fonte da verdade; Sheets e Calendar são espelhos publicados, regeneráveis do zero.
Todo dado bruto (extrato, fatura) entra em `raw/` e nunca é editado; as camadas consolidadas são
geradas por script determinístico e 100% reconstruíveis. O agente propõe, você decide — decisões
viram registro com racional em `plan/decisions.md`, e o agente jamais executa transação
financeira: prepara, agenda e lembra; quem move dinheiro é você.

O onboarding segue as fases F0–F6 descritas em [onboarding/README.md](onboarding/README.md).
A doutrina expandida está em [principios/](principios/) e o contrato do agente em
[AGENTS.md](AGENTS.md).

## FAQ

**Não uso Claude. Funciona?**
Sim. O contrato do agente é o [AGENTS.md](AGENTS.md), formato lido por Codex, Cursor e outros.
O `CLAUDE.md` só redireciona para ele. Qualquer agente que leia arquivos e rode shell serve.

**Não quero conectar o Google.**
Sem problema. Sheets/Calendar são espelhos opcionais; sem eles o agente gera CSVs consolidados e
um dashboard HTML local com as mesmas 6 visões, e a lista de vencimentos vive no dashboard.

**Moro fora do Brasil.**
Os exemplos são brasileiros (Pix, DAS, IPVA, B3, IRPF), porque o sistema de referência nasceu
aqui. A arquitetura — fontes, raw imutável, mandato, espelhos, rotina — é agnóstica de país:
troque as instituições e as obrigações datadas pelas suas.

**Posso usar com meu cônjuge ou contador?**
Sim: compartilhe o repo privado com quem for co-titular ou prestador de confiança. O mandato
registra quem decide; contador entra tipicamente como leitor das fontes PJ e do calendário fiscal.

**Quanto custa?**
O template é MIT, grátis. O custo real é o de tokens do agente que você já usa — o onboarding
consome algumas sessões; a rotina mensal, bem menos.

**Isso é aconselhamento financeiro?**
Não. É uma ferramenta de organização, registro e governança. O agente organiza dados e propõe
análises sob o SEU mandato; toda decisão de investimento é sua e fica registrada como sua.

---

Licença: [MIT](LICENSE).
