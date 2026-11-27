---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer — garanta que essas informações estejam disponíveis. [Arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer, mas também *como* quer que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instrução com escopo de caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Ao contrário dos percursos de VS Code e CLI, você não executará um prompt *antes e depois* neste exercício. O agente de nuvem do Copilot trabalha de forma assíncrona com issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests criados pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser incluída no código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, oferecendo contexto no nível da tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo chat ou CLI, como a stack, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem fornecer diretrizes para linguagens ou tipos de arquivo específicos, como TypeScript ou Astro, ou para tarefas como criar componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para o preenchimento de código nem para sugestões da próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre como criar arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve — e não deve — ser escrito.

Não existe uma única forma de criar arquivos de instruções, assim como não existe uma única forma de usar IA. Com a experimentação, você descobrirá o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre orientações para várias tarefas, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa funcionalidade de uma maneira diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição de cada ambiente indicará o recurso quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob (relativo à raiz do repositório) que determina os arquivos aos quais as instruções se aplicam. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir instruções maiores em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicione um novo padrão ao repositório

Esta será a única edição feita neste exercício: adicione uma regra para todo o projeto que exija documentação no código com comentários TSDoc e um cabeçalho de comentário no arquivo. O agente de nuvem usará essa orientação nas issues atribuídas mais adiante.

Antes de editar, crie uma branch (o agente de nuvem lê as instruções da branch de destino da issue, mas você fará commit das alterações de forma organizada):

1. No terminal do Codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como já vimos, `.github/copilot-instructions.md` fornece orientações no nível do projeto. Vamos registrar os padrões de programação para melhorar as sugestões do Copilot.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela descreve os padrões de programação, mas ainda não define regras para documentação no código.
3. Adicione estas linhas de Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ficar na raiz do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais da equipe, como a regra de comentários que você acabou de adicionar.

## Confirme, envie e mescle a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch visada pela issue. Quando você atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, as instruções precisam ser mescladas em `main` para que o agente de nuvem as use.

1. Prepare e confirme as alterações:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem usará essas instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, ignore as etapas da branch e faça commit diretamente em `main`. A branch serve para reproduzir o processo usado em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem criar nas próximas lições.

Em seguida, você vai [atribuir sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
