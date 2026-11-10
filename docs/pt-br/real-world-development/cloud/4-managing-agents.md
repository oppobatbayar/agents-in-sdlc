---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar as tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente com pull requests abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe decidiu que também seria um bom momento para adicionar um modo claro. Como o trabalho já envolve atualizar os estilos do site e adicionar uma opção de alternância, incluir esse recurso parece razoável. Você orientará o agente para garantir que ele implemente também o modo claro.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento. Revise as operações já executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem que ele adotou.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados para você acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, peça ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você verá mensagens semelhantes aos exemplos.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes, com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará o novo pedido depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso pode levar alguns minutos, então aproveite para refletir sobre o que já aprendeu e explorou.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o local central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Com esse painel, você acompanha tarefas ativas e concluídas, revisa o trabalho e redireciona tarefas em andamento para ajustar o escopo ou fornecer outras orientações.

Você explorou estes conceitos:

- uso do painel de agentes para monitorar as tarefas do agente de nuvem.
- redirecionamento de uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho de acessibilidade, vamos [fazer iterações nos pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
