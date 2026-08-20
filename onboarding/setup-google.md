# Google: autorizar Sheets e Calendar com o gog

Guia para autorizar o Google Sheets e o Google Calendar via [`gog`](https://github.com/openclaw/gogcli),
a primeira rota na ordem de integração de [AGENTS.md](../AGENTS.md). O agente conduz cada passo;
o titular executa os cliques no navegador e aprova os acessos na conta dele.

## Para que serve

A F5 publica a planilha mestra e a agenda de vencimentos; os dois espelhos exigem acesso ao
Google. Sem Google, o fallback local (CSV + `dashboard.html`) cobre tudo, exceto os alertas de
agenda no celular. Este guia é opcional e pode ser feito depois do onboarding: espelhos são
regeneráveis, e autorizar o Google mais tarde exige apenas republicar.

## Custo

10 a 15 minutos. Exige conta Google e passos técnicos no Google Cloud Console. O agente conduz
passo a passo; o titular não precisa conhecer o console.

## Por que um OAuth client próprio

O `gog` não distribui credencial compartilhada: cada titular cria o seu OAuth client. O acesso
fica sob controle do titular: ele concede e revoga na conta Google dele. Os tokens ficam no
keyring do sistema operacional, não em arquivo do repo. Racional: nenhum segredo entra em git, e
nenhum terceiro intermedia o acesso aos dados do titular.

## Passo a passo

Os rótulos de menu do console mudam com o tempo; o agente orienta pela função, não pelo texto
exato.

1. **Criar o projeto.** O titular acessa [console.cloud.google.com](https://console.cloud.google.com)
   e cria um projeto novo (nome livre; sugestão: `wealth-os`).
2. **Habilitar as APIs.** No projeto, em APIs e serviços, Biblioteca: o titular habilita a
   **Google Sheets API** e a **Google Calendar API**.
3. **Configurar a tela de consentimento (OAuth consent screen).** Tipo **Externo**. O titular
   adiciona o próprio e-mail como test user.
4. **Publicar o app (evita reautorizar toda semana).** App Externo em modo de teste (Testing)
   emite refresh token que expira em 7 dias; a rotina mensal quebraria a cada execução. Em
   Audience (Público), o titular clica em **Publish app** e confirma: o app passa a
   "In production". Isso não submete o app à verificação do Google; app pessoal não verificado
   pode operar em produção. Fonte: [quickstart do gog](https://github.com/openclaw/gogcli/blob/main/docs/quickstart.md).
5. **Criar a credencial.** Em Credenciais: o titular cria um **OAuth client ID** do tipo
   **Desktop app** e baixa o JSON.
6. **Instalar o gog.** O titular instala o `gog`. macOS: `brew install openclaw/tap/gogcli`.
   Outros sistemas: instruções no [repositório do gog](https://github.com/openclaw/gogcli).
7. **Registrar a credencial no gog:**

   ```bash
   gog auth credentials set <caminho-do-json>
   ```

8. **Autorizar a conta:**

   ```bash
   gog auth add <email-do-titular> --services sheets,calendar
   ```

   O comando autoriza Sheets, Calendar e também o Drive: o `gog` usa o Drive para listar e
   exportar planilhas, e a tela de consentimento pede esse acesso. Quem quiser Drive mais
   restrito confere `gog auth add --help` (existe a flag `--drive-scope full|readonly|file`)
   e valida os escopos resultantes com `gog auth services` e `gog auth list`. O navegador
   abre; o titular escolhe a conta e aprova. Em sessão sem navegador, `--manual` imprime a URL
   para colar de volta. Se algum comando falhar por sintaxe, confira com `gog auth --help`:
   versões mudam.
9. **Validar.**

   ```bash
   gog auth list
   ```

   A conta deve aparecer com os serviços `sheets` e `calendar`. Complete com uma leitura inócua
   (por exemplo, listar planilhas ou agendas; confira o subcomando com `gog sheets --help` /
   `gog calendar --help`).
10. **Registrar em `plan/ferramentas.md`:** ferramenta, status, escopo autorizado, data (schema
    da F0). A F5 e a rotina mensal leem esse arquivo para decidir a rota de publicação.
11. **Guardar o JSON fora do repo.** Mover o arquivo baixado para fora de `Downloads`, para o
    local onde o titular guarda credenciais, e nunca commitá-lo. O `.gitignore` cobre
    `credentials*.json`, mas o arquivo não deve entrar na pasta do repo.

## Regra operacional

Antes de cada comando de escrita, confira a sintaxe com `gog sheets --help` /
`gog calendar --help`. Versões mudam, e comando não verificado falha na execução. Não invente
flag: se um comando falhar por sintaxe, releia o help.

## Alternativa: MCP

Se a sessão do agente já expõe ferramentas MCP de Google Sheets/Calendar, valide-as com uma
leitura inócua (listar planilhas ou agendas) antes de qualquer escrita. Com o `gog` autorizado, o
MCP é dispensável: a ordem de preferência de [AGENTS.md](../AGENTS.md) coloca o `gog` primeiro.
Registre a rota escolhida em `plan/ferramentas.md`.
