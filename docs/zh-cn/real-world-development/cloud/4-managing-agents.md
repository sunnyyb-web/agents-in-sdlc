---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot cloud agent 承担了三个改善用户体验和添加功能的任务。Cloud agent 虽然可以异步、自主工作，但监控任务同样重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个 mission control 中，你可以查看所有具有未关闭 PR 的代理任务、探索代理执行的操作，并在会话进行中引导其工作。

本课将学习：

- 使用代理页面监控 cloud agent 任务。
- 在会话进行中引导代理添加功能。

## 场景

为团队添加高对比度模式后，团队意识到也应提供浅色模式。由于代理已经在更新网站样式并添加切换功能，因此同时实现浅色模式也很合适。你将引导代理，使它同时添加浅色和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的所有任务：

1. 访问 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表，其中应包括更新代码文档、生成用于修改游戏的 API，以及为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如检出代码、创建工作环境、设置 MCP 服务器，以及完成分配任务的步骤。

> [!NOTE]
> 实际显示的步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 查看右侧显示的拉取请求面板，其中可以看到 PR 和已更改文件。

## 引导 cloud agent

现在你已查看正在运行的任务，可以要求 Copilot 在实现高对比度模式的同时添加浅色模式。

1. 选择用于添加高对比度模式的会话。标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

   ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到代理完成设置并开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![代理页面中的云端代理任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成

Copilot 会继续处理更新后的任务。完成当前步骤后，它会将新请求纳入工作流程。整个过程可能需要几分钟，可以借此时间回顾已学内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控并引导 Copilot cloud agent 任务的中心位置。你可以跟踪正在进行和已完成的任务、审查代理所做的工作，还可以在会话中途重新设定范围或补充指导。

本课中，你：

- 使用 mission control 和代理页面监控 cloud agent 任务。
- 在会话进行中要求代理添加功能。

Copilot 正在完成功能实现。接下来，开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
