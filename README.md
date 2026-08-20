# wealth-os

> **seu family office, operado pelo seu agente**

**wealth-os** is a public, whitelabel GitHub template for running your financial life out of a
private git repo, operated by the AI agent you already use (Claude Code, Codex, Cursor, or
similar). Copy the template into a private repo, open your agent in the folder, and say
*"faz meu onboarding"*. The agent interviews you, builds your investment mandate, consolidates
statements, publishes a master spreadsheet and a due-date calendar, and installs a monthly
financial routine. Everything below is in Brazilian Portuguese; the architecture is
country-agnostic.

---

## O pitch

Sua vida financeira hoje depende de instrumentos que não resolvem o problema:

- **Planilha manual.** Você monta uma vez, alimenta por alguns meses e abandona. Digitar dado na
  mão toda semana não se sustenta como sistema.
- **Apps de banco.** Um silo por instituição, e cada um mostra só o que aquela instituição quer
  vender. Nenhum responde "quanto sobra por mês?" nem "estou no caminho da minha meta?".
- **Consultor ou family office.** Resolve, mas cobra caro e em geral exige um patrimônio mínimo
  que a maioria das pessoas não tem.

Um repo git com um agente de IA resolve melhor, por três razões:

1. **O agente absorve o trabalho operacional.** Classificar fatura, consolidar extrato, projetar
   fluxo de caixa, republicar planilha e agenda: o agente executa esse trabalho de forma
   consistente. Você só exporta os extratos e toma as decisões.
2. **O repo é a fonte da verdade; planilha e agenda são espelhos.** Tudo fica versionado em git,
   no seu repo privado. Você audita qualquer número até o extrato de origem. Você pode apagar a
   planilha e regenerá-la idêntica. Nada é digitado duas vezes.
3. **Governança de family office, ao custo do agente que você já usa.** Mandato escrito (IPS),
   decisões registradas com racional, rotina mensal de 30 a 45 min, comitê trimestral. É o
   protocolo de um family office, sem o custo de contratar um.

O template não traz nenhum dado: só protocolo, templates e princípios. Seus dados entram apenas
no seu repo privado e permanecem lá.

## Quickstart em 4 passos

1. **Crie seu repo privado.** Clique em **"Use this template"** no GitHub e marque **Private**
   (ou clone e suba para um repo privado seu). Privado é obrigatório: este repo vai conter seus
   dados financeiros.
2. **Traga o repo para a sua máquina.** No seu repo privado, clique em **Code**, copie o
   endereço e rode `git clone <endereço>` no terminal. Se preferir não usar terminal, use o
   GitHub Desktop (Code > Open with GitHub Desktop). Entre na pasta criada.
3. **Abra seu agente na pasta.** `claude`, `codex`, ou qualquer agente que leia arquivos e rode
   shell.
4. **Diga: "faz meu onboarding".** O agente conduz a partir daí: entrevista, extratos, orçamento,
   patrimônio, planilha, agenda, rotina. Leva de 2 a 5 sessões e é retomável. Pare quando quiser;
   o agente detecta onde parou e continua.

## O que você ganha ao final

Cada fase entrega valor sozinha e destrava a seguinte. Não é preciso concluir todas as fases
para ter resultado.

| Fase | O que acontece | O que você leva |
|------|----------------|-----------------|
| **F0: Preparação** | checagem de privacidade, detecção de ferramentas, esqueleto do repo | repo seguro e pronto para operar |
| **F1: Mandato** | entrevista: sua meta, risco, liquidez, tetos, alocação-alvo | `plan/mandato.md`, seu IPS escrito |
| **F2: Fontes e extratos** | inventário de contas, cartões, corretora, cripto, imóveis, dívidas | toda fonte documentada + 12 meses de dados brutos |
| **F3: Orçamento** | renda, fixos, obrigações datadas, taxonomia e classificação de gastos | orçamento real + primeiro fluxo de caixa projetado |
| **F4: Patrimônio** | snapshot de balanço: ativos, passivos, PF e PJ | patrimônio líquido datado, comparado à alocação-alvo |
| **F5: Planilha e agenda** | publicação dos espelhos | planilha mestra (6 abas) + agenda com alertas de vencimento |
| **F6: Rotina e governança** | instalação das cadências e registro de riscos | rotina mensal de 30 a 45 min + comitê trimestral agendado |

## Requisitos

- **Um agente de IA com acesso a arquivos e shell.** Claude Code, Codex CLI, Cursor, ou
  equivalente. O template é agnóstico: as instruções vivem em [AGENTS.md](AGENTS.md), formato
  que se tornou convenção entre os principais agentes.
- **Opcional: Google Sheets e Calendar**, via CLI `gog` ou MCP, para a planilha mestra e a
  agenda de vencimentos com alertas.
- **Sem Google, o template funciona do mesmo modo:** o agente gera CSVs consolidados e um
  dashboard HTML local com as mesmas visões. A planilha é espelho, não fonte; por isso é
  dispensável.

## Privacidade

- **Seus dados nunca saem do seu repo privado.** O template é público; a sua cópia é privada e
  desconectada dele. Não existe backend, telemetria ou coleta. Nenhum dado retorna ao template.
- O onboarding começa (F0) confirmando que o repo é privado e que `.gitignore` cobre `.env` e
  chaves. O agente checa `git remote -v` antes de qualquer push.
- **O agente nunca pede senha de banco.** Você exporta os extratos no site do banco e os coloca
  no repo. Cripto entra apenas por endereço público.
- Doutrina completa e checklist: [principios/privacidade.md](principios/privacidade.md).

## Como funciona por dentro

O repo é a fonte da verdade; Sheets e Calendar são espelhos publicados, regeneráveis do zero.
Todo dado bruto (extrato, fatura) entra em `raw/` e ninguém o edita. Scripts determinísticos
geram as camadas consolidadas, reconstruíveis do zero. O agente propõe; você decide. Cada
decisão vira registro com racional em `plan/decisions.md`. O agente nunca executa transação
financeira: ele prepara, agenda e lembra. Quem move dinheiro é você.

O onboarding segue as fases F0 a F6 descritas em [onboarding/README.md](onboarding/README.md).
A doutrina expandida está em [principios/](principios/) e o contrato do agente em
[AGENTS.md](AGENTS.md).

## FAQ

**Não uso Claude. Funciona?**
Sim. O contrato do agente é o [AGENTS.md](AGENTS.md), formato lido por Codex, Cursor e outros.
O `CLAUDE.md` só redireciona para ele. Qualquer agente que leia arquivos e rode shell serve.

**Não quero conectar o Google.**
Não é necessário. Sheets e Calendar são espelhos opcionais. Sem eles, o agente gera CSVs
consolidados e um dashboard HTML local com as mesmas 6 visões; a lista de vencimentos fica no
dashboard.

**Moro fora do Brasil.**
Os exemplos são brasileiros (Pix, DAS, IPVA, B3, IRPF) porque o sistema de referência nasceu no
Brasil. A arquitetura (fontes, raw imutável, mandato, espelhos, rotina) é agnóstica de país.
Troque as instituições e as obrigações datadas pelas suas.

**Posso usar com meu cônjuge ou contador?**
Sim. Compartilhe o repo privado com quem for co-titular ou prestador de confiança. O mandato
registra quem decide; o contador entra tipicamente como leitor das fontes PJ e do calendário
fiscal.

**Quanto custa?**
O template é MIT, gratuito. O custo é o de tokens do agente que você já usa. O onboarding
consome algumas sessões; a rotina mensal consome bem menos.

**Isso é aconselhamento financeiro?**
Não. É uma ferramenta de organização, registro e governança. O agente organiza dados e propõe
análises sob o seu mandato; toda decisão de investimento é sua e fica registrada como sua.

---

Licença: [MIT](LICENSE).
