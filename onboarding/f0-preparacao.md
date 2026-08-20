# F0: Preparação

Fase de fundação: garante que o ambiente é seguro e conhecido antes de qualquer dado pessoal
entrar no repo. Nenhum dado financeiro entra nesta fase. F0 termina somente quando o repo é
comprovadamente privado, as ferramentas disponíveis estão registradas e o esqueleto de
`plan/` existe em git.

**Entrega da fase:** repo privado confirmado, `plan/ferramentas.md`, `plan/perfil.md`,
`plan/proximos-passos.md`, `plan/perguntas-abertas.md`, primeiro commit.
**Gate para F1:** todos os artefatos acima existem e o titular confirmou o contrato de
privacidade. A confirmação de privacidade é obrigatória: se o repo estiver público, interrompa
o onboarding neste ponto.

## Passo 1. Privacidade: confirmar que o repo é privado

Regra: nenhum dado do titular entra no repo antes desta confirmação. Racional: o template de
origem é público. O erro típico é preencher dados no lugar errado ou com o remote errado.
Histórico de git não se apaga de forma confiável depois de um push.

Execute e interprete:

```bash
git remote -v
```

| Resultado | Leitura | Ação |
|---|---|---|
| Sem remote | Repo apenas local, a opção mais privada | OK; avisar que backup é responsabilidade do titular |
| Remote próprio (GitHub/GitLab) | Precisa confirmar visibilidade | `gh repo view --json visibility -q .visibility` (se `gh` existir); senão, pedir ao titular que confira na página do repo |
| Remote apontando para o template público (`wealth-os`) | Errado: qualquer push vazaria dados | Remover o remote (`git remote remove origin`) e orientar a criar repo privado próprio |

Se a visibilidade for `PUBLIC`: parar, instruir o titular a tornar o repo privado (no GitHub:
Settings, Danger Zone, Change visibility; ou `gh repo edit --visibility private`) e seguir
somente depois de re-verificar.

Confirme também o `.gitignore`: ele deve cobrir `.env`, `.env.*`, `*.key` e tokens. Se o
titular criou o repo por outro caminho e o `.gitignore` não existe, copie o do template antes
de qualquer outra coisa. Racional: credencial commitada é credencial vazada; revogar depois
não a remove do histórico.

**Contrato de privacidade. Diga ao titular, nestas 3 frases (ou equivalente):**

1. Seus dados ficam neste repositório privado, na sua máquina e no seu git. Nada volta para o
   template público nem vai para terceiros.
2. Eu nunca peço senha de banco nem acesso às suas contas: você exporta os extratos e eles
   entram aqui como arquivos.
3. Antes de qualquer push eu confiro o remote. `.env` e chaves nunca entram em commit.

Peça confirmação explícita ("podemos seguir?") antes do Passo 2. Detalhes e checklist completo
em [../principios/privacidade.md](../principios/privacidade.md).

## Passo 2. Detectar ferramentas disponíveis

Regra: nunca assumir que uma integração existe. Testar e registrar. Racional: o onboarding
tem dois espelhos opcionais (Google Sheets e Google Calendar); descobrir na F5 que a
ferramenta não funciona desperdiça a sessão. O sistema funciona 100% sem Google
(CSV + dashboard HTML local); a detecção não bloqueia, apenas define o caminho.

Teste nesta ordem e pare no primeiro que funcionar por serviço:

```bash
which gog        # CLI Google: Sheets e Calendar
gog auth list    # contas autorizadas e escopos concedidos
```

| Ferramenta | Como detectar | Observação |
|---|---|---|
| `gog` CLI | `which gog` + `gog auth list` | Verificar o escopo: Sheets e Calendar são autorizações separadas |
| MCP de Sheets/Calendar | Inspecionar as ferramentas expostas na sua sessão | MCP de terceiro vê o que você envia; o alerta de privacidade se aplica |
| Fallback local | Sempre disponível | `plan/consolidado/*.csv` + `dashboard.html`; nada a instalar |

Registre o resultado em `plan/ferramentas.md`. Schema simples, uma linha por ferramenta:

```markdown
# Ferramentas

| Ferramenta | Status | Escopo autorizado | Data |
|---|---|---|---|
| gog | ok | sheets, calendar | 2026-08-19 |
| MCP Sheets | ausente | n/a | 2026-08-19 |
| Fallback local | disponível | csv + html | 2026-08-19 |
```

Racional do registro: as fases F5-F6 leem este arquivo para decidir o caminho de publicação
sem re-testar, e o contexto sobrevive à troca de agente (de Claude para outro).

## Passo 3. Esqueleto do plano

Crie a estrutura mínima de dados do titular (o template traz apenas protocolo e moldes; os
diretórios de dados são criados agora):

1. `mkdir plan/`
2. Copiar [../templates/proximos-passos.md](../templates/proximos-passos.md) →
   `plan/proximos-passos.md`
3. Copiar [../templates/perguntas-abertas.md](../templates/perguntas-abertas.md) →
   `plan/perguntas-abertas.md`

Racional: o registro de pendências precisa existir desde a primeira pergunta. Todo
"não sei / depois" do titular tem local de registro imediato, e o onboarding não é
interrompido por falta dele.

## Passo 4. Perfil do titular

Entrevista curta, **uma pergunta por vez** (registre a resposta antes da próxima):

1. **Nome / como prefere ser chamado?**
2. **Moeda-base?** (default: BRL; toda consolidação converte para ela)
3. **País e fuso?** (default: Brasil, `America/Sao_Paulo`; o sistema assume Brasil nos
   exemplos, mas a arquitetura é agnóstica de país)
4. **Dia preferido da rotina mensal?** Sugestão: **dia 5**. Racional: no dia 5 o mês anterior
   já fechou (extratos e faturas disponíveis) e ainda há folga antes do bloco de vencimentos
   do dia 10. A rotina alimenta os alertas em vez de rodar depois deles.

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
   ([f1-mandato.md](f1-mandato.md)), de 45 a 60 minutos, que pode ocorrer em outra sessão.

Checklist de saída (gate):

- [ ] Repo privado confirmado (ou local sem remote) e `.gitignore` cobrindo `.env`
- [ ] Contrato de privacidade dito e confirmado pelo titular
- [ ] `plan/ferramentas.md` com o resultado real da detecção
- [ ] `plan/proximos-passos.md` e `plan/perguntas-abertas.md` criados dos templates
- [ ] `plan/perfil.md` preenchido
- [ ] Commit `f0: preparação` feito
