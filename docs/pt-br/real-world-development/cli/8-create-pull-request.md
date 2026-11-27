---
title: "Lição 8 - Crie e mescle o PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise tudo em conjunto e abra um pull request. Você mesclou o PR de classificação por estrelas manualmente; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos a funcionalidade, as atualizações das instruções e da skill e o agente de QA em alguns pull requests. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma conversa e branch e incluiremos o trabalho em um único PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionar a alteração completa e as evidências de verificação.
- criar o PR da filtragem.
- ativar o Agent Merge somente depois da revisão e confirmar que o PR foi mesclado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo o controle da autorização de mesclagem com a pessoa desenvolvedora.

## Conheça o Agent Merge

O **Agent Merge** automatiza o trabalho restante necessário para concluir um pull request. Quando ativado, o Copilot resolve os bloqueios do PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e ativa a mesclagem automática do GitHub se o repositório permitir.

Até aqui, você selecionou **Merge pull request** manualmente. O Agent Merge pode cuidar desse processo. Revise o trabalho e confirme que está pronto antes de ativar o Agent Merge.

## Use o Agent Merge para gerenciar o PR

Com o código pronto, vamos revisá-lo em conjunto, criar o PR e deixar o Agent Merge gerenciar as próximas etapas.

1. Volte ao Codespace.
2. Abra a caixa de diálogo de agentes digitando `/agent`.
3. Selecione **Default** na lista e pressione <kbd>Enter</kbd>.
4. Crie um novo PR usando o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua do PR. Quando todas as verificações forem aprovadas, ele fará a mesclagem.
7. Confirme que o Copilot apresenta uma mensagem semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de mesclagem, configurações do repositório nem permissões ausentes. Se o processo for bloqueado, leia o motivo informado e, quando o repositório permitir, faça manualmente a mesclagem revisada.

## Resumo e próximos passos

Você automatizou várias etapas do processo de desenvolvimento: geração de código, teste e validação e, agora, o fluxo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida da mesclagem.
- inspecionou a alteração completa e as evidências de verificação.
- criou o PR de filtragem.
- ativou o Agent Merge somente depois da revisão e confirmou a mesclagem.

Em seguida, você vai [explorar outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
