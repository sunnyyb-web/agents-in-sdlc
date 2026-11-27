---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并检查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支中。现在可以一并检查并创建拉取请求。你之前手动合并了星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 实际工作中，通常会把功能、说明更新、技能更新和 QA 代理拆分为几个 PR。为了简化练习，这些内容都保留在同一个对话和分支中，并通过一个 PR 提交。

本课将学习：

- 了解 Agent Merge 以及它如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 完成检查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的开发过程中，你使用 Copilot 规划、实现并验证了功能。现在，Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

此前你是自己选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍须检查更改并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码都已完成，现在可以一起检查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请查看报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

你自动化了开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。你：

- 了解了 Agent Merge 及其如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 检查后启用了 Agent Merge，并确认 PR 已合并。

接下来，你将探索 Copilot CLI 中其他有用的斜杠命令，以管理上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
