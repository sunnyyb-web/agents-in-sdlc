---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

使用 GitHub Copilot agent mode 时，代理会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联查看差异、接受或拒绝单项更改，并通过后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将学习：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导代理，在高对比度模式之外添加浅色模式切换。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作期间，设计团队提出了额外需求：为觉得默认主题太暗的用户添加浅色模式切换。与其从头开始新对话，不如引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异的形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 使用的 Copilot Chat 视图。
2. 代理工作期间滚动聊天内容，查看每次工具调用，包括读取文件、编辑文件和请求运行的终端命令。
3. 代理提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让代理重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中代理修改的全部文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式实现。完成时会显示更改摘要。

## 引导代理添加浅色模式

高对比度模式实现后，在同一对话中要求增加浅色模式切换。由于继续使用同一会话，代理会保留前面工作的上下文，包括修改的组件、样式方法和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 代理会检查已有更改，并提出添加浅色模式的方案。
3. 与高对比度模式相同，逐项检查新的编辑，并选择 **Keep** 或 **Undo**。
4. 如果代理的方向不符合预期，例如试图以不希望的方式重构切换界面，可选择聊天输入框旁的 **Stop** 按钮停止任务，然后发送澄清提示。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以用后续提示要求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，请将浅色模式集成到现有设置面板”），或扩大范围（“为两个开关添加键盘快捷方式”）。

## 检查工作集

提交前快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查已更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并查看差异。确认自定义代理采用了 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化等无障碍模式。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat agent mode 监控并引导进行中的会话：检查每次文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课中，你学习了：

- 实时监控代理的工作。
- 接受或拒绝单项文件编辑。
- 通过后续提示在会话进行中引导代理。

在[下一课][next-lesson]中，你会将无障碍工作发布为拉取请求，并审查本练习中本地自定义代理生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用 agent mode][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
