# Privacidade: o contrato de segurança do sistema

Este documento expande a doutrina 8. Ela é a última da lista e a primeira na execução: a F0
([`../onboarding/f0-preparacao.md`](../onboarding/f0-preparacao.md)) executa este checklist
**antes** de qualquer dado entrar no repo, e nenhuma fase avança com ele pendente.

## O modelo em uma frase

O **template** (wealth-os) é público e não contém dado algum. O **repo do titular**, criado a
partir do template, é privado e contém todos os dados. Os dados fluem em uma direção só: do mundo
para o repo privado. Nunca voltam: nem ao template, nem a serviço algum além dos espelhos que o
titular autorizou.

**Racional:** um repositório financeiro pessoal é o alvo mais valioso que um indivíduo pode vazar
sobre si mesmo: saldos, dívidas, endereços, rotina. A defesa não é sofisticação, é disciplina:
poucos pontos de vazamento possíveis, cada um com uma regra simples e verificável.

## Checklist de segurança (contrato: F0 executa, rotina reverifica)

| # | Regra | Racional |
|---|---|---|
| 1 | **Repo privado, sempre.** Antes de qualquer push: `git remote -v` e confirmar que o remote é privado. Sem remote (repo só local) também é válido. | Um push errado para remote público publica todo o histórico financeiro do titular. É o pior acidente possível do sistema; a checagem que o previne leva segundos. |
| 2 | **`.env` e credenciais fora do git.** O `.gitignore` do template já cobre `.env`, `.env.*`, `*.key`; é proibido relaxar essas linhas. | Credencial commitada permanece no histórico. Remover o arquivo depois não remove o commit. |
| 3 | **Chave nunca em commit; chave exposta é rotacionada.** Se uma key ou token aparecer em qualquer commit, rotacionar imediatamente na origem. Apagar o arquivo não basta. | O histórico do git preserva o segredo, e qualquer clone antigo o carrega. Rotacionar é a única correção efetiva. |
| 4 | **Cripto: só endereços públicos.** Endereço público de carteira pode entrar no repo (é observável on-chain de qualquer forma). Seed phrase, chave privada e arquivo de keystore **nunca** entram, em nenhum arquivo, nem temporariamente. | Endereço público lê o saldo; chave privada **move** o saldo. Um dá visibilidade; o outro dá o dinheiro. |
| 5 | **MCPs e ferramentas de terceiros: escopo mínimo, entendimento prévio.** Antes de autorizar qualquer integração (MCP, CLI, API), saber o que ela envia para fora e para quem. Autorizar só o escopo necessário (ex.: Sheets sem Gmail). Registrar o que foi autorizado em `plan/ferramentas.md`. | Cada integração é um canal de saída de dados. O risco não é a ferramenta ser maliciosa; é ela logar, sincronizar ou treinar com o que vê. Escopo mínimo limita o dano de qualquer uma. |
| 6 | **O template nunca recebe dados de volta.** Nenhum PR, issue ou fork público a partir do repo privado. Melhorias no template vão limpas, reescritas do zero, sem nenhum arquivo do titular. | O caminho do repo privado para o público é o único em que um vazamento é irreversível e indexável. A regra elimina o caminho em vez de confiar no cuidado. |
| 7 | **O agente nunca pede senha de banco; o extrato é exportado pelo titular.** O agente instrui como exportar (passo a passo no README da fonte) e processa o arquivo que o titular trouxe. Nunca pede senha, token de acesso bancário, código de SMS ou frase de segurança. Se qualquer fluxo pedir, é sinal de erro: parar e revisar. | Senha bancária na mão de um agente quebra o modelo de responsabilidade (doutrina 5: quem move dinheiro é o titular) e cria um segredo impossível de auditar. Exportar leva minutos e mantém a credencial com o titular. |

## O que o agente verifica, e quando

- **Na F0:** itens 1, 2 e 5, antes do primeiro commit com dado real. O agente explica o contrato
  de privacidade ao titular em três frases e registra as ferramentas autorizadas em
  `plan/ferramentas.md`.
- **Em toda sessão que fizer push:** item 1 de novo (`git remote -v`). Remotes mudam; a checagem
  é barata e o acidente não é.
- **Ao integrar fonte nova (F2) ou ferramenta nova:** itens 4, 5 e 7. Toda fonte cripto entra
  por endereço público; toda ferramenta nova entra com escopo mínimo e registro.
- **Se encontrar violação** (credencial em commit, remote público, seed em arquivo): parar o que
  estiver fazendo, avisar o titular com o achado exato e o passo de correção (item 3), e retomar
  só depois da correção. Vazamento não entra em backlog; é a única classe de problema que
  interrompe qualquer fase.

## O que este sistema NUNCA faz

Resumo das proibições. Cada uma já apareceu acima; juntas formam o contrato que o titular pode
cobrar de qualquer agente em qualquer sessão:

- Nunca pede senha, token bancário ou código de autenticação.
- Nunca executa transação financeira (ver [`governanca.md`](governanca.md), doutrina 5).
- Nunca commita segredo, seed ou chave privada.
- Nunca faz push sem confirmar que o remote é privado.
- Nunca envia dados do titular a serviço que não esteja registrado em `plan/ferramentas.md`.
- Nunca devolve dado nenhum ao template público.
