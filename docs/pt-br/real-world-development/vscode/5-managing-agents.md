---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina e em tempo real, o Copilot Chat no VS Code exibe cada chamada de ferramenta e edição de arquivo. Você pode revisar cada diff, aceitar ou rejeitar alterações e orientar a conversa com prompts complementares, sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um botão de alternância de modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade que você usou no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design fez uma solicitação adicional: também gostaria de um modo claro para quem acha o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe cada chamada de ferramenta. Você verá leituras e edições de arquivos e comandos do terminal solicitados.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra também o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando do terminal), o agente pausará e exibirá o prompt **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, vamos estender a mesma conversa para adicionar um botão de alternância do modo claro. Como você continuará na mesma sessão, o agente mantém o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o uso do armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho, explorará as alterações existentes e proporá adições para o modo claro.
3. Revise cada nova edição conforme ela aparecer. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das opções de alternância — pare-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o que deseja.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro botão; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione atalhos de teclado para os dois modos").

## Revise o conjunto de alterações

Antes de confirmar o trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá encontrar atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns dos arquivos alterados e revise os diffs. Confirme se as práticas de acessibilidade do agente personalizado estão presentes: atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e confirme as alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o Agent Mode do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
