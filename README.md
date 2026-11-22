# orchestrator

Skill para o [Claude Code](https://claude.com/claude-code) que transforma a sessão atual em **sessão pai** e coordena a implementação de vários tickets por sessões filhas: mapeia dependências, executa em paralelo o que é independente, valida cada entrega e mantém a comunicação com o usuário centralizada numa única conversa.

## O que a skill faz

- Monta um grafo de dependências (DAG) entre os tickets e inicia só os que estão liberados.
- Cria uma sessão filha por ticket (via Agent Teams, quando disponível), com o contexto mínimo necessário.
- Isola arquivos e áreas por ticket, usando worktree/branch quando possível, para evitar edições concorrentes.
- Centraliza dúvidas e decisões na sessão pai; o usuário só fala diretamente com uma filha quando ela depende de uma ação que só ele pode executar ali (por exemplo, preencher algo no navegador da filha).
- Só marca um ticket como concluído depois de revisar testes e critérios de aceite.
- Relata com precisão as limitações da ferramenta, sem simular sessões ou monitoramento que não existam.

## Instalação

Copie a pasta `orchestrator` para o diretório de skills do Claude Code.

Para todos os projetos (pasta do usuário):

```bash
git clone https://github.com/<seu-usuario>/orchestrator-skill.git
cp -r orchestrator-skill/orchestrator ~/.claude/skills/
```

Para um projeto específico, copie para `.claude/skills/orchestrator` dentro do repositório.

## Uso

A skill tem `disable-model-invocation: true`, então só roda quando chamada explicitamente:

```text
/orchestrator T01, T02 e T03 — T03 depende de T01
```

Tudo o que vier depois de `/orchestrator` chega à skill como `$ARGUMENTS`: lista de tickets, caminhos de arquivos com as especificações ou instruções livres.

## Estrutura

```text
orchestrator/
  SKILL.md   # instruções da skill
```

## Licença

[MIT](LICENSE)
