# orchestrator

Skill para o [Claude Code](https://claude.com/claude-code) que transforma a sessão atual em **sessão pai** e coordena a implementação de vários tickets por sessões filhas: mapeia dependências, executa em paralelo o que é independente, valida cada entrega e mantém a comunicação com o usuário centralizada numa única conversa.

## O que a skill faz

- Monta um grafo de dependências (DAG) entre os tickets e uma branch de integração de onde todas as filhas nascem.
- Abre **uma subsessão por ticket logo no início**, inclusive as bloqueadas. Cada filha estuda o ticket em leitura e começa assim que recebe da pai o `LIBERADO`, mandado quando todos os predecessores são aceitos.
- Isola arquivos e áreas por ticket (worktree/branch) e empresta recursos exclusivos a uma filha por vez.
- Centraliza as perguntas: a filha cria a `PERGUNTA` e manda à pai. A pai pesquisa nos tickets, specs, diretrizes e decisões anteriores; responde sozinha só o que essas fontes decidem de forma explícita, citando a fonte, e leva todo o resto ao mantenedor numa fila única.
- Notifica o mantenedor a cada nova leva de perguntas, pelo canal informado na chamada (uma skill ou um hook).
- Só marca um ticket como concluído depois de conferir diff, testes e critérios de aceite.
- Relata com precisão as limitações da ferramenta, sem simular sessões ou monitoramento que não existam.

## Instalação

Copie a pasta `orchestrator` para o diretório de skills do Claude Code.

Para todos os projetos (pasta do usuário):

```bash
git clone https://github.com/DevLucasLourenco/orchestrator-skill.git
cp -r orchestrator-skill/orchestrator ~/.claude/skills/
```

Para um projeto específico, copie para `.claude/skills/orchestrator` dentro do repositório.

## Uso

A skill tem `disable-model-invocation: true`, então só roda quando chamada explicitamente:

```text
/orchestrator T01, T02 e T03 — T03 depende de T01
```

Para ser avisado quando houver perguntas, informe o canal na chamada:

```text
/orchestrator T01, T02 e T03 — T03 depende de T01. notificar: /minha-skill-de-aviso
/orchestrator .scratch/mapas/issues — notificar: hook
```

- `notificar: /<skill>`: a pai invoca a skill com os ids e a quantidade de perguntas (o conteúdo fica na conversa). A skill de aviso não pode ter `disable-model-invocation: true`, senão a pai não consegue chamá-la.
- `notificar: hook`: a pai termina o turno com a fila de perguntas, e o hook que você configurou (por exemplo, em `Stop` ou `Notification`) dispara.
- Sem canal, as perguntas aparecem só na conversa da sessão pai.

Tudo o que vier depois de `/orchestrator` chega à skill como `$ARGUMENTS`: lista de tickets, caminhos de arquivos com as especificações, instruções livres e o canal de notificação.

## Estrutura

```text
orchestrator/
  SKILL.md         # procedimento da sessão pai
  brief-filha.md   # mensagem inicial de cada sessão filha
```

## Licença

[MIT](LICENSE)
