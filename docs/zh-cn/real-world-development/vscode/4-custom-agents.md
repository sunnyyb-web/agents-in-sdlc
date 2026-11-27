---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，并提供专门的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定工作。团队可以将专业知识整理成可复用代理，例如检查 [WCAG][wcag] 合规性的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少带有 `name` 和 `description` 的 YAML frontmatter，以及用于定义代理行为、专业能力和指令的 Markdown 提示。

### 自定义代理与 Agent Skills 的比较

自定义代理和 [Agent Skills][agent-skills-concept] 有一些重叠：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义代理是执行任务的工作者，技能则是工具**。

自定义代理拥有独立的上下文窗口，可在工作中协调技能，甚至协调其他代理。本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景的对比度不足。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此你将使用自定义代理尽快生成所需功能。

本练习将学习：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat agent mode 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已为无障碍工作创建了自定义代理。返回 Codespace，在终端中从 `main` 创建新分支，用于无障碍功能，与练习 3 的筛选 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解无障碍网站代码生成的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 目录里的自定义代理。选择代理后，会使用该代理的说明和工具限定聊天会话。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新对话。模式和模型选择因练习而异；每个步骤会在适用时说明。

1. 如果代理下拉菜单中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认活动代理为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并应用到项目文件。每项编辑都会以内联方式显示在聊天中，包含文件路径和差异供你审查。
6. 随着编辑应用，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等项目文件。

下一课将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，它们是针对特定任务或领域定制的 AI 助手。使用自定义代理可以将团队专业知识和标准整理成可复用指南，帮助 Copilot 更有效地完成特定工作。

本课中，你：

- 了解了如何定义自定义代理。
- 在 Copilot Chat agent mode 中使用了自定义代理。

接下来，你将[监控并引导代理工作][next-lesson]，在更改进行时检查它们，并要求代理在同一会话中添加浅色模式切换。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [Awesome Copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
