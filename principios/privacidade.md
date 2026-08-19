# Privacidade — o contrato de segurança do sistema

Este documento expande a doutrina 8. Ela vem por último na lista e primeiro na prática: a F0
([`../onboarding/f0-preparacao.md`](../onboarding/f0-preparacao.md)) executa este checklist
**antes** de qualquer dado entrar no repo, e nenhuma fase avança com ele pendente.

## O modelo em uma frase

O **template** (wealth-os) é público e não contém dado nenhum; o **seu repo**, criado a partir
dele, é privado e contém todos. Dados fluem numa direção só — do mundo para o seu repo privado —
e **nunca voltam**: nem ao template, nem a serviço nenhum além dos espelhos que você autorizou.

**Racional:** um repositório financeiro pessoal é o alvo mais valioso que um indivíduo pode vazar
sobre si mesmo — saldos, dívidas, endereços, rotina. A defesa não é sofisticação, é disciplina:
poucos pontos de vazamento possíveis, cada um com uma regra simples e verificável.

## Checklist de segurança (contrato — F0 executa, rotina reverifica)

| # | Regra | Racional |
|---|---|---|
| 1 | **Repo privado, sempre.** Antes de QUALQUER push: `git remote -v` e confirmar que o remote é privado. Sem remote (repo só local) também é válido. | Um push errado para remote público publica sua vida financeira inteira, com histórico. É o pior acidente possível do sistema e custa 5 segundos prevenir. |
| 2 | **`.env` e credenciais fora do git.** O `.gitignore` do template já cobre `.env`, `.env.*`, `*.key`; nunca relaxar essas linhas. | Credencial commitada fica no histórico para sempre — remover o arquivo depois não remove o commit. |
| 3 | **Chave nunca em commit — e chave vazada é chave morta.** Se uma key/token aparecer em qualquer commit, rotacionar imediatamente na origem; não basta apagar o arquivo. | O histórico do git não esquece, e qualquer clone antigo carrega o segredo. Rotacionar é a única correção real. |
| 4 | **Cripto: só endereços públicos.** Endereço público de carteira pode entrar no repo (é observável on-chain de qualquer forma). Seed phrase, chave privada e arquivo de keystore **nunca**, em nenhum arquivo, nem "temporariamente". | Endereço público lê saldo; chave privada MOVE o saldo. Um dá visibilidade, o outro dá o dinheiro. |
| 5 | **MCPs e ferramentas de terceiros: escopo mínimo, entendimento prévio.** Antes de autorizar qualquer integração (MCP, CLI, API), saber o que ela envia para fora e para quem; autorizar só o escopo necessário (ex.: Sheets sem Gmail). Registrar o que foi autorizado em `plan/ferramentas.md`. | Cada integração é um canal de saída de dados. O risco não é a ferramenta ser maliciosa — é ela logar, sincronizar ou treinar com o que vê. Escopo mínimo limita o estrago de qualquer uma. |
| 6 | **O template nunca recebe dados de volta.** Nenhum PR, issue ou fork público a partir do seu repo privado; melhorias no template vão limpas, reescritas do zero, sem nenhum arquivo seu. | O caminho privado→público é o único em que um vazamento é irreversível e indexável. A regra elimina o caminho, não confia no cuidado. |
| 7 | **O agente NUNCA pede senha de banco — extrato é exportado pelo titular.** O agente instrui *como* exportar (passo a passo no README da fonte) e processa o arquivo que o titular trouxe. Nunca pede senha, token de acesso bancário, código de SMS ou frase de segurança; se qualquer fluxo pedir, é sinal de erro — parar e revisar. | Senha bancária na mão de um agente quebra o modelo inteiro de responsabilidade (doutrina 5: quem move dinheiro é o titular) e cria um segredo impossível de auditar. Exportar leva minutos e mantém a credencial onde ela deve estar: só com o dono. |

## O que o agente verifica, e quando

- **Na F0:** itens 1, 2 e 5 — antes do primeiro commit com dado real. O agente explica o contrato
  de privacidade ao titular em três frases e registra as ferramentas autorizadas em
  `plan/ferramentas.md`.
- **Em toda sessão que fizer push:** item 1 de novo (`git remote -v`). Remotes mudam; a checagem
  é barata e o acidente, não.
- **Ao integrar fonte nova (F2) ou ferramenta nova:** itens 4, 5 e 7 — toda fonte cripto entra
  por endereço público; toda ferramenta nova entra com escopo mínimo e registro.
- **Se encontrar violação** (credencial em commit, remote público, seed em arquivo): parar o que
  estiver fazendo, avisar o titular com o achado exato e o passo de correção (item 3), e só
  retomar depois de corrigido. Vazamento não é pendência de backlog — é a única classe de
  problema que interrompe qualquer fase.

## O que este sistema NUNCA faz

Resumo executável das linhas vermelhas — cada uma já apareceu acima, mas juntas formam o contrato
que o titular pode cobrar de qualquer agente em qualquer sessão:

- Nunca pede senha, token bancário ou código de autenticação.
- Nunca executa transação financeira (ver [`governanca.md`](governanca.md), doutrina 5).
- Nunca commita segredo, seed ou chave privada.
- Nunca faz push sem confirmar que o remote é privado.
- Nunca envia dados do titular a serviço que não esteja registrado em `plan/ferramentas.md`.
- Nunca devolve dado nenhum ao template público.
