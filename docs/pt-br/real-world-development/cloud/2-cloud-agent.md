---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações não enfrentam dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades esquecidas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) de rascunho para registrar o progresso, permitindo que uma pessoa desenvolvedora revise o resultado. Assim, você pode delegar tarefas demoradas e se concentrar em questões mais importantes.

Neste exercício, você vai explorar como:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- delimitar issues com clareza.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver algumas dívidas técnicas. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para que as pessoas continuem explorando o catálogo. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testar a experiência. Embora essa tarefa esteja bloqueada, há issues de prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são boas candidatas para o agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você cuida de outras tarefas e, depois, você pode revisar o resultado.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Você pode iniciar o trabalho de várias formas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um PR de rascunho para acompanhar o progresso, prepara um ambiente e começa a trabalhar. Você pode acompanhar a sessão durante o trabalho ou depois da conclusão. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções bem delimitadas

O GitHub Copilot não é mágico: não elimina a necessidade de pensar. Até operações aparentemente simples ficam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu a atividade prática de instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para dar uma olhada. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes para observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface web do GitHub ou em um codespace:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no pull request. Essas orientações vêm dos arquivos de instrução. Voltaremos a esse tema ao revisar os PRs em um exercício futuro.

## Configure o ambiente de desenvolvimento do agente de nuvem

O desenvolvimento de código costuma exigir um ambiente específico e scripts de configuração para instalar dependências e preparar os serviços. Isso também se aplica ao Copilot, que executa as tarefas de maneira semelhante a uma pessoa desenvolvedora de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso disponibiliza as ferramentas e dependências necessárias. O fluxo foi pré-configurado para este laboratório: ele instala o Node.js e as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para estas etapas. O Copilot receberá seu próprio token para as operações.
    permissions:
      # Se as etapas de configuração clonarem o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Caso contrário, o Copilot fará isso automaticamente depois da conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo tem a estrutura de um fluxo de trabalho normal do GitHub, mas há dois detalhes importantes:

- Ele contém um único trabalho, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions**. Assim, você pode testá-lo sem esperar que o Copilot inicie a tarefa.

## Adicione documentação ao código

Muitos projetos têm documentação desatualizada ou inexistente. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas na equipe. O Copilot pode ajudar a criar documentação, então essa é uma boa issue para atribuir ao agente de nuvem. Você revisará o trabalho em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo PR, cujo título será parecido com `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o fluxo de trabalho descrito anteriormente), analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do PR até encontrar uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Acompanhe a sessão em tempo real. O Copilot explorará o código, analisará o estado atual, planejará a abordagem e criará o código.

Esse trabalho pode levar alguns minutos. Uma das vantagens do agente de nuvem é executar tarefas de forma assíncrona, enquanto você cuida de outros trabalhos. Vamos aproveitar esse recurso e atribuir outra tarefa ao agente; depois, passaremos a implementar funcionalidades no aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Vamos atribuir ao Copilot a tarefa de adicionar uma seção de jogos relacionados para a equipe de design.

1. Volte ao repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá o emoji 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a uma pessoa desenvolvedora, você não precisa acompanhá-lo ativamente: revise os resultados quando estiverem prontos. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para que ele trabalhe de forma assíncrona. Ele pode resolver dívidas técnicas, criar funcionalidades ou ajudar na migração de código entre frameworks.

Você explorou estes tópicos:

- personalizar o ambiente para gerar código.
- garantir que as operações sejam realizadas com segurança.
- delimitar issues com clareza.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, vamos criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas do fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
