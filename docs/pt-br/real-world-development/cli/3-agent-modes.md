---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar a filtragem a partir de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho existente da organização e ajudar a desenvolver as soluções certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com uma issue para gerar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará a mesclagem no projeto.

> [!NOTE]
> Você usará a mesma conversa e branch durante todo o fluxo de trabalho do recurso. Normalmente, você usaria branches ou pull requests diferentes para cada tipo de arquivo, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso usando o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador com porta encaminhada.

Ao continuar este trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com MCP, criará um agente de QA e abrirá o pull request do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser acordados antes de programar. Você usará o modo Plan para definir essas decisões e, depois, autorizará uma implementação delimitada no Autopilot.

## Contexto

Introduzir agentes de programação com IA no fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante a este:

1. Comece com uma issue registrada que descreva o trabalho necessário.
2. Crie um plano do que precisa ser desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois que o código for revisado e a integração contínua for aprovada, faça a mesclagem.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue uma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação do código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR de classificação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir no contexto uma issue ou um pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem de implementação antes de delegar o trabalho ao Copilot. É sempre uma boa ideia reservar algum tempo para planejar tarefas complexas. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, insira este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a explorar o projeto e a definir a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role o conteúdo para revisar o plano.
6. Peça ao Copilot para revisar qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere até considerar o problema resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes da mesclagem. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot para corrigir as falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que o recurso funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e as combinações definidas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot para parar o servidor de desenvolvimento que iniciou.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador com porta encaminhada.

Em seguida, vamos aprofundar como o código é gerado e garantir que siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
