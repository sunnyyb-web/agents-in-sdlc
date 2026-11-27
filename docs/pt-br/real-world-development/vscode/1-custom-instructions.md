---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer — garanta que essas informações estejam disponíveis. Há várias formas de fornecer contexto ao Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que orientam a geração de código.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instrução com escopo de caminho.
- gerar a primeira parte dos dados de filtragem (um helper de editoras) com as instruções *atuais*.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado seguir o novo padrão.
- confirmar as instruções e a parte da filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns padrões definidos. O Copilot é probabilístico. O objetivo é observar a *tendência* de mudança de comportamento após atualizar as instruções, não obter exatamente a mesma saída em todos os caracteres.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser incluída no código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, oferecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo Copilot, como a stack, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem oferecer diretrizes para linguagens ou tipos de arquivo específicos, como TypeScript ou Astro, ou para tarefas como criar componentes de interface e testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para geração de código no Copilot Chat — não para preenchimento de código nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as instruções claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não existe uma única forma de criar arquivos de instruções, assim como não existe uma única forma de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre orientações para várias tarefas, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa funcionalidade de maneira diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em que você estiver indicará o recurso quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob (relativo à raiz do repositório) que determina os arquivos aos quais as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Crie uma branch para as alterações

Vamos seguir boas práticas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam de acordo com o exercício; cada etapa indicará as opções relevantes.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. Na lista de agentes da visualização Chat, selecione **Agent** para que o Copilot possa aplicar alterações aos arquivos.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt a seguir:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explora o projeto e aplica as atualizações, muitas vezes incluindo o helper e os testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele inclua comentários de documentação mesmo sem essa orientação. Isso não é um problema: o objetivo é observar a *consistência* depois da atualização das instruções.

## Adicione um novo padrão ao repositório

Como vimos, `.github/copilot-instructions.md` fornece orientações no nível do projeto. Vamos registrar os padrões de programação para melhorar as sugestões do Copilot.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela descreve os padrões de programação, mas ainda não inclui documentação no código.
3. Logo abaixo dos padrões existentes, adicione estas linhas para pedir cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, para fornecer orientações globais, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais da equipe, como a regra de documentação que você acabou de adicionar.

## Reenvie o prompt e observe a diferença

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código que será usado no próximo exercício.

## Confirme as instruções e envie a branch

Os arquivos de instruções são gerenciados pelo controle de versão como qualquer outro arquivo. Vamos confirmar as alterações e enviá-las ao repositório.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme se as atualizações de instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e confirme as atualizações de instruções e os arquivos alterados pelo Copilot para o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se o git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione também esses arquivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Depois de enviar a branch, crie um pull request associado à issue de padrões de documentação do backlog. O Copilot pode encontrar a issue, criar um PR que a encerra e, depois, mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot age no GitHub por você. O modelo do projeto já conecta as ferramentas do GitHub ao workspace, então não é preciso configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code talvez peça que você entre no GitHub. Siga as instruções para autorizar. Você aprenderá sobre essa conexão (Model Context Protocol) em uma lição futura.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot para localizar a issue relacionada à atualização dos arquivos de instruções e criar um PR a partir da branch atual, descrevendo também o helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot pedir permissão para executar ações do GitHub, revise o comando e selecione **Allow** se for apropriado.
5. Depois da criação do PR, peça ao Copilot para mesclá-lo e voltar para `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell, revise-os e selecione **Allow** se for apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código seguir o novo padrão.
- confirmar as instruções e o helper em `main` para que o próximo exercício possa usá-los.

Em seguida, você usará essas instruções no [modo de agente][next-lesson] para adicionar uma funcionalidade em vários arquivos.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
