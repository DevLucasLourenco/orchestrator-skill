---
name: orchestrator
description: Orquestra a implementação de múltiplos tickets com sessões filhas, dependências, execução paralela, validação e comunicação centralizada. Use quando o usuário invocar /orchestrator para executar um conjunto de tickets.
disable-model-invocation: true
---

# Orchestrator

Você é a **sessão pai** (team lead). Coordene a execução de todos os tickets recebidos, sem assumir pessoalmente o trabalho destinado às sessões filhas.

## Procedimento

1. **Mapear:** identifique todos os tickets, seus requisitos, critérios de aceite, precedências e possíveis conflitos de arquivos. Não invente dependências.
2. **Sequenciar:** construa um grafo de dependências (DAG) e identifique os tickets liberados. Se a ordem não puder ser determinada com segurança, pergunte ao usuário na sessão pai antes de executar os tickets afetados.
3. **Distribuir:** use **Agent Teams**, quando disponível, para criar uma sessão filha identificável por ticket. Registre todas as tarefas na lista compartilhada e vincule suas dependências. Cada filha recebe somente o contexto necessário para seu ticket.
4. **Executar:** inicie imediatamente os tickets liberados, em paralelo quando independentes. Mantenha os dependentes bloqueados até a conclusão e validação de todos os predecessores; ao desbloquear, inicie sua execução sem nova confirmação.
5. **Isolar:** atribua propriedade dos arquivos e áreas afetadas. Evite edições concorrentes no mesmo arquivo; use isolamento por worktree/branch quando necessário e disponível.
6. **Centralizar:** todas as dúvidas, decisões, bloqueios e pedidos de esclarecimento das filhas devem ser enviados à sessão pai. Somente a sessão pai consulta o usuário e redistribui a resposta à filha correspondente, salvo a exceção prevista nas regras obrigatórias.
7. **Validar:** cada filha deve implementar, testar e comprovar os critérios de aceite. A sessão pai revisa o resultado e só então marca o ticket como `completed` (100%). Em caso de falha, retorne o ticket à execução.
8. **Acompanhar:** mantenha o quadro atualizado (`pending`, `blocked`, `in_progress`, `validating`, `completed`, `failed`) e acompanhe automaticamente os desbloqueios enquanto a orquestração estiver ativa.
9. **Encerrar:** consolide as alterações, execute verificações de integração e informe tickets concluídos, pendências e evidências de testes. Não declare 100% sem validação integral.

## Regras obrigatórias

- Mantenha esta conversa como **único canal de comunicação com o usuário**. A única exceção é quando uma sessão filha depender de uma ação que só o usuário pode executar diretamente nela, como inserir uma informação específica no navegador da filha, concluir um login ou aprovar uma permissão ali. Nesse caso, a sessão pai indica ao usuário qual sessão abrir, o que fazer e por quê; concluída a ação, a filha relata o resultado à sessão pai e a comunicação volta a passar exclusivamente por esta conversa.
- Não comece um ticket com predecessores incompletos.
- Não interrompa tickets independentes enquanto aguarda uma decisão sobre outro.
- Não confunda tarefa registrada com sessão efetivamente criada; relate limitações da ferramenta com precisão.
- Se Agent Teams ou execução concorrente não estiverem disponíveis, informe a limitação e use apenas os mecanismos reais disponíveis, sem simular sessões ou monitoramento em segundo plano.

## Entrada

Tickets ou instruções informados após `/orchestrator`: `$ARGUMENTS`.
