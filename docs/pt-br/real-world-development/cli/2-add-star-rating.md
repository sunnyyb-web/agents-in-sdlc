---
title: "Lição 2 - Adicione a classificação por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cartões de jogos, revisá-la no navegador com porta encaminhada e mesclá-la no primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, chegou a hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma classificação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a classificação, revisará a alteração e a mesclará no seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de funcionalidade.
- pedir ao agente para fazer uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- iniciar o aplicativo e confirmar a alteração em um navegador com porta encaminhada.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma classificação por estrelas, que já aparece na página de detalhes. Os cartões da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir a classificação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a discussão; os arquivos e a branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá seus prompts e as respostas do agente, as atividades do agente enquanto explora e edita arquivos e as alterações que pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para desenvolver o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Peça a alteração com este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explorará o projeto, localizará os arquivos que exibem os detalhes dos jogos e criará o código necessário. Você acabou de adicionar um recurso com o Copilot CLI.

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes de serem mescladas, mesmo quando são pequenas. Vamos inspecionar as alterações no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão mostra a classificação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça que corrija qualquer falha.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot para iniciar o site e examine a interface atualizada no navegador com porta encaminhada pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões exibem a classificação dos jogos de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou o recurso. Agora, crie um pull request (PR) para mesclar o código no projeto.

1. Peça ao agente padrão para confirmar a alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e exibe a URL do PR.
3. Para abrir o PR, mantenha <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecione a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada em uma branch de funcionalidade.
- orientou o agente para fazer uma pequena alteração nos cartões.
- revisou a alteração com `/diff`.
- iniciou o aplicativo e confirmou a classificação por estrelas em um navegador com porta encaminhada.
- abriu e mesclou seu primeiro pull request.

Em seguida, você vai [começar pela issue de filtragem e usar os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
