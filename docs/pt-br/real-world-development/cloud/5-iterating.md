---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot para documentar o código, criar uma seção de jogos relacionados para a equipe de design testar e implementar recursos de acessibilidade, incluindo opções para alternar entre os modos de alto contraste e claro. Vamos explorar as alterações e, se necessário, enviar comentários para que o Copilot aprimore o resultado.

### Cenário

Como já destacamos, os fundamentos do design de software e do DevOps não mudam com a inclusão da IA generativa. Sempre devemos revisar o código gerado e seguir nosso processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, existem restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Os fluxos de trabalho do GitHub Actions exigem aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo o acesso a servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Você o revisará pela interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez apareça um aviso de que o GitHub Copilot está bloqueado por um firewall. Isso é **esperado**: por padrão, o Copilot tem acesso limitado a recursos externos, incluindo servidores MCP. Se quiser, você pode [personalizar ou desativar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho; pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui os comentários TSDoc recém-criados e outras documentações. As alterações exatas variam.

   Ao revisar as alterações, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Eles vêm dos arquivos de instruções personalizados que você examinou no início do Exercício 2.

6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila de verificações do PR. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários associados a trechos do código, como faria com outra pessoa da equipe. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados não são determinísticos, não há um texto único para usar. Você pode pedir, por exemplo:

- adicionar um cabeçalho de comentário no início de cada arquivo de código com uma breve descrição.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Comente uma alteração na documentação gerada e mencione **@copilot**, como faria com qualquer pessoa. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione a guia **Files changed** para revisar as mudanças.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos voltar ao PR criado pelo Copilot para a issue sobre jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Você verá um indicador de que há fluxos de trabalho aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila de verificações do PR. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** mude para a branch do Copilot no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, **copilot/fix-8**):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot implementou a seção de jogos relacionados. Como antes, você pode iterar com o agente de nuvem e pedir atualizações. Por exemplo, solicite que ajuste a quantidade de jogos exibidos ou adicione cabeçalhos de comentário e comentários TSDoc (lembre-se: a issue foi atribuída **antes** da atualização das instruções personalizadas). Para isso, adicione um comentário na guia **Conversation**; o Copilot verá a mensagem e iniciará outra sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deverá incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no centro de controle do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS e estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância entre modos.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Você verá um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila de verificações do PR. Se tudo estiver correto, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** mude para a branch do Copilot no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos a seguir (substitua `<branch-name>` pelo nome da branch criada pelo Copilot):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Depois, inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme que as preferências persistem ao recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo boas práticas. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar atualizações, como nos outros PRs.

## Exercício opcional — continue delegando

O agente de nuvem é mais útil quando você pode delegar itens reais do backlog e cuidar de outras tarefas. Para desenvolver esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizados do repositório** para observar como eles influenciam o trabalho do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para adicionar modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade.
- **Revisou e iterou sobre pull requests do agente de nuvem**, mencionando `@copilot` para solicitar mudanças e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar aprendendo, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o Agent Mode do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, Playwright MCP, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
