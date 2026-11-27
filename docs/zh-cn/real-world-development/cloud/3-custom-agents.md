---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，并提供专门的指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定工作。团队可以将专业知识整理成可复用代理，例如检查 [WCAG][wcag] 合规性的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少带有 `name` 和 `description` 的 YAML frontmatter，以及用于定义代理行为、专业能力和指令的 Markdown 提示。

### 自定义代理与 Agent Skills 的比较

自定义代理和 [Agent Skills][agent-skills-concept] 有一些重叠：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义代理是执行任务的工作者，技能则是工具**。

自定义代理拥有独立的上下文窗口，可在工作中协调技能，甚至协调其他代理。本练习中，无障碍自定义代理会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一的正确写法。与其他 AI 用法一样，应在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义代理，你将学习：

- 如何定义自定义代理。
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景的对比度不足。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此你将使用自定义代理尽快生成所需功能。

## 查看无障碍自定义代理

项目已为无障碍工作创建了自定义代理。返回 Codespace，查看其内容：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解无障碍网站代码生成的核心职责、最佳实践、HTML/CSS/JavaScript 示例，以及常见问题。

## 创建并分配问题

Mission control 是管理环境中所有代理的中心位置。你可以在此将任务分配给 Copilot cloud agent、监控任务并引导进行中的会话。现在将添加高对比度模式的任务分配给 Copilot。

1. 访问存储库，选择 **Issues** 选项卡，然后选择 **New issue** 和 **Blank issue**。
2. 将 **Title** 设置为 `Add high contrast mode to website`。
3. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. 选择 **Create** 创建问题。
5. 在右侧选择 **Assign to Copilot**，再从自定义代理列表中选择 **Accessibility agent**，然后选择 **Assign**。

   ![Copilot 云端代理分配界面，突出显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 自定义代理][custom-agents]，它们是针对特定任务或领域定制的 AI 助手。使用自定义代理可以将团队专业知识和标准整理成可复用指南，帮助 Copilot 更有效地完成特定工作。

本课中，你：

- 了解了如何定义自定义代理。
- 将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，前往 mission control[监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [为组织使用自定义代理做准备][org-custom-agents]
- [为企业使用自定义代理做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
