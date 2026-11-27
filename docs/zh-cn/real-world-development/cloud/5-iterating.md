---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作成果

在本练习中，你让 GitHub Copilot 完成了多项改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式等无障碍功能。现在来检查代码更改，并在需要时向 Copilot 提供反馈，改进它的工作。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍须审查生成的代码，并遵循正常的 DevOps 流程。接下来，在为团队其他成员开启审查前，先检查 Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全和 GitHub Copilot cloud agent

Copilot cloud agent 会异步、无人监督地执行任务，因此设置了一些安全约束：

- Copilot 只有存储库读取权限，并且只对用于编写代码的分支具有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经人工批准后才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 PR：为代码添加文档。通过 GitHub.com 上的标准 PR 界面完成检查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 选项卡，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并检查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能有所不同。检查是否包含 TSDoc、TypeScript 约定和文件头注释；这些要求来自练习 2 中查看的自定义说明文件。
6. 检查完成后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流。如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 的检查区域中。确认单体 Astro 应用的项目检查通过。此过程可能需要几分钟。

## 要求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像 @ 团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话进行处理。由于生成结果并非确定性结果，我们无法提供唯一固定的提示。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 在 PR 中添加评论并提及 **@copilot**，要求它修改生成的文档。可以使用上述建议，或提出其他代码文档改进意见。
2. 选择 **View Session**，查看 Copilot 如何启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡检查更改。

可以继续迭代，直到满意为止。之后，可将 PR 从草稿转换为准备审查状态，并合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来检查 Copilot 为“在游戏详情页显示相关游戏”问题创建的 PR。

1. 返回 GitHub.com 上的存储库，选择 **Pull Requests** 选项卡。
2. 打开标题类似 **Show related games on the game details page** 的 PR。
3. 选择 **Files changed** 选项卡，检查生成的代码。
4. 检查完成后，返回 **Conversation** 选项卡并向下滚动。
5. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

6. 在 PR 的检查区域确认单体 Astro 应用的项目检查通过。此过程可能需要几分钟。
7. **可选：** 在 Codespace 中切换到该分支，手动测试相关游戏功能。将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已完成相关游戏功能。你可以继续迭代并要求更新，例如调整显示的相关游戏数量，或添加文件头注释和 TSDoc 文档注释。注意：分配该任务时，你尚未更新自定义说明。与前面一样，可在 **Conversation** 选项卡添加评论，让 Copilot 根据评论启动新会话。

## 审查无障碍功能

最后，检查通过自定义无障碍代理实现的功能。PR 应同时包含练习 3 中要求的高对比度模式，以及练习 4 中在 mission control 里提出的浅色模式。

1. 返回 GitHub.com 上的存储库并选择 **Pull Requests** 选项卡。
2. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待它完成，这可能需要几分钟。

3. 选择 **Files changed** 选项卡检查生成的代码。
4. 检查实现，尤其关注：
   - 用于切换模式的界面组件。
   - 使用本地存储保留用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性，例如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。
5. 检查完成后，返回 **Conversation** 选项卡并向下滚动。如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

6. 在 PR 的检查区域确认单体 Astro 应用的项目检查通过。
7. **可选：** 在 Codespace 中切换到此分支，手动测试无障碍功能：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍保留设置。

注意，自定义无障碍代理帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可像处理其他 PR 一样在评论中提及 **@copilot** 并要求更新。

## 可选练习 — 继续委派

Cloud agent 适合处理真实待办事项，让你可以专注于其他工作。可在存储库中创建更多问题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 练习。在这些课程中，你：

- **检查了存储库随附的自定义说明文件**，了解它们如何影响 cloud agent 的输出。
- **向 Copilot cloud agent 分配了问题**，并观察它异步设置环境、制定计划和执行工作。
- **创建并使用自定义代理**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为 mission control**，监控并引导无障碍会话。
- **审查并迭代 cloud agent 的拉取请求**，提及 `@copilot` 要求更改并批准工作流。

## 回顾和后续步骤

Cloud agent 练习已完成。其他练习环境可以补充本次所学内容：

- 🖥️ **[VS Code 练习](../../vscode/)** — 直接在 IDE 中体验 Copilot Chat agent mode 和 MCP 集成。
- 💻 **[CLI 练习](../../cli/)** — 在终端中发布两项经过审查的更改，使用 Plan 和 Autopilot 模式、Agent Skills、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云代理桥接。

还可以在自己的存储库中尝试以下内容：

- 将重构或测试覆盖率问题分配给 cloud agent。
- 为其他领域创建自定义代理。
- 针对不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot agents][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
