---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas voltadas a melhorar a experiência do usuário. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho em um navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — controles para alternar entre os modos de alto contraste e claro — e orientou a sessão em andamento para ampliar o trabalho. Agora é hora de publicar essas alterações locais e revisá-las da mesma forma que sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a adição de IA generativa. Você ainda precisa de um ciclo real de revisão de tudo o que o Copilot produz. Com isso em mente, vamos enviar as alterações de acessibilidade do codespace ao repositório remoto, abrir um pull request e examinar o diff antes de envolver o restante da equipe.
## Publicar os recursos de acessibilidade

Os controles dos modos de alto contraste e claro que você implementou com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no codespace como alterações com commit. Vamos enviá-las em uma branch e abrir um pull request para que o restante da equipe possa revisá-las.

1. Volte ao codespace.
2. Abra a exibição **Source Control** no VS Code.
3. Confirme se as alterações de acessibilidade foram incluídas em um commit. Se houver alterações sem commit do Exercício 5, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro (por exemplo, `Add high-contrast and light-mode toggles`) e uma descrição breve que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois que o PR estiver aberto, selecione a guia **Files changed** para revisar o trabalho de ponta a ponta. Preste atenção especial a:
   - componentes da interface com controles para alternar entre os modos;
   - uso do armazenamento local para manter as preferências do usuário;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.);
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

9. Retorne à guia **Conversation**.
10. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)
11. Aguarde a conclusão dos fluxos de trabalho. Se tudo correr bem, eles deverão passar.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR com uma solicitação, como "revise este PR para encontrar outros problemas relacionados à WCAG" ou "sugira melhorias para a navegação pelo teclado". O Copilot iniciará uma nova sessão para atender ao comentário.

## Exercício opcional — continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que se desenvolve com a prática. Experimente estas ideias em outras sessões no VS Code:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados em `src/lib/`;
- ampliar o escopo do agente de acessibilidade, por exemplo, para auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu a trilha do VS Code! Ao longo deste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Adicionou o servidor MCP do Playwright e permitiu que o Copilot controlasse um navegador para verificar o recurso de filtragem antes da abertura do pull request.
- **Orientou o modo de agente durante alterações coordenadas em toda a pilha.** Em uma única sessão, adicionou um recurso de filtragem que afetou o cliente, o servidor e os testes.
- **Usou um agente personalizado.** Selecionou o agente personalizado voltado à acessibilidade no seletor de agentes e observou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas diretamente no editor, aceitou o que queria e ampliou a sessão com uma solicitação adicional de modo claro.
- **Concluiu o fluxo com um pull request.** Publicou o trabalho local e o revisou de ponta a ponta como faria com a equipe.

## Revisão e próximos passos

Isso encerra a trilha obrigatória do VS Code. Você pode parar aqui com o workshop concluído.

Se quiser ampliar sua perspectiva sobre os recursos de agente do Copilot, as outras trilhas abordam cenários relacionados em interfaces diferentes:

- **[Roteiro do CLI](../../cli/)** — entregue duas alterações revisadas no terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e transições de contexto.
- **[Roteiro do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, monitorar sessões na página de agentes e iterar de forma assíncrona nos pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
