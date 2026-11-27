---
title: "Exercício 2 - Adicione uma funcionalidade com o Agent Mode"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam envolver vários arquivos e operações, como executar testes. Em geral, as pessoas desenvolvedoras precisam localizar os arquivos, fazer as alterações, executar os testes, depurar, identificar o que ficou faltando e atualizar o código novamente. Esse processo pode continuar por várias etapas.

É aí que entra o Agent Mode do Copilot.

O Agent Mode foi criado para trabalhar com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar uma funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos de feedback rápidos e interação direta. O agente de nuvem funciona de forma assíncrona, como uma pessoa parceira da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o Agent Mode explora o projeto, identifica os arquivos relevantes e coordena as alterações.
- o Agent Mode implementa funcionalidades na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo Agent Mode antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um helper de editoras. Agora, terminará o trabalho restante na camada de dados, na interface e nos testes com o Agent Mode.

## Execute o site da Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades existentes.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir os detalhes de cada um. O site é um único aplicativo Astro, que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local pelo frontmatter usando o Drizzle ORM. Não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno elemento `<script>` do Astro que usa APIs padrão do DOM.

### Inicie o site

O projeto oferece o comando `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Execute-o no GitHub Codespaces:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespaces, selecionar o link local no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda o servidor web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam de acordo com o exercício; cada etapa indicará as opções relevantes.

A primeira versão do site funciona, mas queremos adicionar recursos. Vamos começar revisando o backlog. A criação do projeto a partir do modelo incluiu automaticamente um conjunto de issues. Peça ao GitHub Copilot para exibi-las.

1. Na lista de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos no terminal e usa ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não recomendam um modelo em particular. Usamos o Claude Sonnet 4.5 ao criar o workshop e o incluímos para que as sugestões sejam relativamente consistentes. Como os LLMs são probabilísticos, os resultados podem ser diferentes dos exemplos; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie este prompt para perguntar sobre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com a utilidade para quem usa o aplicativo.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos, incluindo orientações para Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir instruções complexas em partes menores e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou atualizar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes também devem ser executados e aprovados antes da mesclagem. O Agent Mode pode realizar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Seleção do arquivo de instruções da interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista de agentes da visualização Chat.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados, incluindo helpers da camada de dados, interface e testes. Depois, ele modificará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as alterações; não precisa fazer isso enquanto o trabalho ainda está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. Isso é normal: o Copilot alterna entre geração de código e testes até concluir a tarefa e não detectar erros.

   ![Sessão concluída do Copilot Agent Mode no Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Lembre-se de que é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, você usou o GitHub Copilot Agent Mode para adicionar funcionalidades ao site da Tailspin Toys. Você aprendeu como:

- o Agent Mode implementa funcionalidades na interface e na camada de dados.
- o Agent Mode explora o projeto, identifica os arquivos relevantes e coordena alterações.
- revisar as alterações e os testes gerados pelo Agent Mode antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus de exploração — implemente paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use as habilidades deste exercício para pedir ao Copilot que implemente essa funcionalidade. Considere:

- seguir as práticas existentes, incluindo o uso dos arquivos de instruções.
- decidir como a paginação deve funcionar, por exemplo, se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer no prompt as orientações necessárias para que o Copilot implemente a solução desejada.
- iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos Ask, Edit e Agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
