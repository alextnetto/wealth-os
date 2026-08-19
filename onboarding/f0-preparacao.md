# F0 — Preparação

Fase de fundação: garante que o ambiente é seguro e conhecido antes de qualquer dado pessoal
entrar no repo. Nada de finanças ainda — F0 só termina quando o repo é comprovadamente privado,
as ferramentas disponíveis estão registradas e o esqueleto de `plan/` existe em git.

**Entrega da fase:** repo privado confirmado, `plan/ferramentas.md`, `plan/perfil.md`,
`plan/proximos-passos.md`, `plan/perguntas-abertas.md`, primeiro commit.
**Gate para F1:** todos os artefatos acima existem e o titular confirmou o contrato de
privacidade. Privacidade não é passo opcional: se o repo estiver público, o onboarding PARA aqui.

## Passo 1 — Privacidade: confirmar que o repo é privado

Regra: nenhum dado do titular entra no repo antes desta confirmação. Racional: o template de
origem é público; o erro clássico é preencher dados no lugar errado ou com o remote errado, e
histórico de git não se apaga de forma confiável depois de um push.

Execute e interprete:

```bash
git remote -v
```

| Resultado | Leitura | Ação |
|---|---|---|
| Sem remote | Repo só local — o mais privado possível | OK; avisar que backup é responsabilidade do titular |
| Remote próprio (GitHub/GitLab) | Precisa confirmar visibilidade | `gh repo view --json visibility -q .visibility` (se `gh` existir); senão, pedir ao titular que confira na página do repo |
| Remote apontando para o template público (`wealth-os`) | ERRADO — qualquer push vazaria dados | Remover o remote (`git remote remove origin`) e orientar a criar repo privado próprio |

Se a visibilidade for `PUBLIC`: parar, instruir o titular a tornar o repo privado
(Settings → Danger Zone → Change visibility, ou `gh repo edit --visibility private`) e só
seguir depois de re-verificar.

Confirme também o `.gitignore`: ele deve cobrir `.env`, `.env.*`, `*.key` e tokens. Se o
titular criou o repo por outro caminho e o `.gitignore` não existe, copie o do template antes
de qualquer outra coisa. Racional: credencial commitada é vazada — revogar depois não
desfaz o histórico.

**Contrato de privacidade — diga ao titular, nestas 3 frases (ou equivalente):**

1. Seus dados ficam neste repositório privado, na sua máquina e no seu git — nada volta para o
   template público nem vai para terceiros.
2. Eu nunca peço senha de banco nem acesso às suas contas: extratos são exportados por você e
   entram aqui como arquivos.
3. Antes de qualquer push eu confiro o remote, e `.env`/chaves nunca são commitados.

Peça confirmação explícita ("podemos seguir?") antes do Passo 2. Detalhes e checklist completo
em [../principios/privacidade.md](../principios/privacidade.md).

## Passo 2 — Detectar ferramentas disponíveis

Regra: nunca assumir que uma integração existe — testar e registrar. Racional: o onboarding
tem dois espelhos opcionais (Google Sheets e Google Calendar); descobrir na F5 que a
ferramenta não funciona desperdiça a sessão. O sistema funciona 100% sem Google
(CSV + dashboard HTML local), então a detecção nunca bloqueia — só define o caminho.

Teste nesta ordem e pare no primeiro que funcionar por serviço:

```bash
which gog        # CLI Google — Sheets e Calendar
gog auth list    # contas autorizadas e escopos concedidos
```

| Ferramenta | Como detectar | Observação |
|---|---|---|
| `gog` CLI | `which gog` + `gog auth list` | Verificar ESCOPO: Sheets e Calendar são autorizações separadas |
| MCP de Sheets/Calendar | Inspecionar as ferramentas expostas na sua sessão | Cuidado: MCP de terceiro vê o que você envia — vale o alerta de privacidade |
| Fallback local | Sempre disponível | `plan/consolidado/*.csv` + `dashboard.html`; nada a instalar |

Registre o resultado em `plan/ferramentas.md` — schema simples, uma linha por ferramenta:

```markdown
# Ferramentas

| Ferramenta | Status | Escopo autorizado | Data |
|---|---|---|---|
| gog | ok | sheets, calendar | 2026-08-19 |
| MCP Sheets | ausente | — | 2026-08-19 |
| Fallback local | disponível | csv + html | 2026-08-19 |
```

Racional do registro: as fases F5–F6 leem este arquivo para decidir o caminho de publicação
sem re-testar; e a troca de agente (Claude → outro) não perde o contexto.

## Passo 3 — Esqueleto do plano

Crie a estrutura mínima de dados do titular (o template só traz protocolo e moldes; os
diretórios de dados nascem agora):

1. `mkdir plan/`
2. Copiar [../templates/proximos-passos.md](../templates/proximos-passos.md) →
   `plan/proximos-passos.md`
3. Copiar [../templates/perguntas-abertas.md](../templates/perguntas-abertas.md) →
   `plan/perguntas-abertas.md`

Racional: pendência estruturada precisa existir DESDE a primeira pergunta — qualquer
"não sei / depois" do titular já tem onde morar, e nada trava o onboarding por falta de lugar
para registrar.

## Passo 4 — Perfil do titular

Entrevista curta, **uma pergunta por vez** (registre a resposta antes da próxima):

1. **Nome / como prefere ser chamado?**
2. **Moeda-base?** (default: BRL — toda consolidação converte para ela)
3. **País e fuso?** (default: Brasil, `America/Sao_Paulo` — o sistema assume Brasil nos
   exemplos, mas a arquitetura é agnóstica de país)
4. **Dia preferido da rotina mensal?** Sugestão: **dia 5**. Racional: no dia 5 o mês anterior
   já fechou (extratos e faturas disponíveis) e ainda há folga antes do bloco de vencimentos
   do dia 10 — a rotina alimenta os alertas em vez de chegar atrasada a eles.

Grave em `plan/perfil.md`:

```markdown
# Perfil

- Nome / como chamar: <nome>
- Moeda-base: <BRL>
- País: <Brasil>
- Fuso: <America/Sao_Paulo>
- Dia da rotina mensal: <5>
```

## Encerramento da fase

1. Commit único: `f0: preparação` (esqueleto + ferramentas + perfil).
2. Anuncie ao titular o que existe agora e o que vem a seguir: a entrevista de mandato
   ([f1-mandato.md](f1-mandato.md)), ~45–60 min, pode ser em outra sessão.

Checklist de saída (gate):

- [ ] Repo privado confirmado (ou local sem remote) e `.gitignore` cobrindo `.env`
- [ ] Contrato de privacidade dito e confirmado pelo titular
- [ ] `plan/ferramentas.md` com o resultado real da detecção
- [ ] `plan/proximos-passos.md` e `plan/perguntas-abertas.md` criados dos templates
- [ ] `plan/perfil.md` preenchido
- [ ] Commit `f0: preparação` feito
