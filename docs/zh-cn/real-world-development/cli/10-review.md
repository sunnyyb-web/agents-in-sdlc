---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续开发工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选问题。
- 添加星级评分，在转发浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选问题出发，在 Plan 模式中制定方案，在 Autopilot 模式中实现，并在 Interactive 模式下检查结果。
- 使用自定义说明指导代理，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证（QA）自定义代理，检查需求、覆盖率、技能结果和浏览器证据。
- 检查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 你交付的内容

本练习有两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流**：实现筛选、更新说明并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为了简化练习，我们把相关工作合并到一个 PR 中。

## 不同类型的验证

你使用多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 浏览器探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 代理在 PR 创建前综合检查这些结果、需求和测试覆盖率。

新增的测试应弥补实际缺口；如果 QA 检查后无需添加测试，也可能是合理结果。授权合并前，请检查代码和证据；代码更改后应重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本练习中，你更新了说明、自定义技能、创建 QA 配置并设置 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专业角色，MCP 服务器则连接外部工具。请检查实际更改和工具结果，而不只是代理的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 执行范围清晰的任务。常规编辑可选择速度更快的模型；复杂工作则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建**什么**、**为什么**需要，以及**如何**实现，会显著影响输出。

## 继续探索

你已掌握核心工作流。还可以继续了解一些 CLI 功能：

- `/review`：让代码审查代理分析更改。
- `/rubber-duck`：讨论问题并获取另一种看法。
- `/fleet`：并行协调互相独立的子任务。
- `/worktree`：隔离单独任务。
- `/delegate`：将任务交给 Copilot cloud agent。

## 后续步骤

持续使用工具是提升技能的好方法！将它用于生产代码、个人项目，或多年来一直想做的小应用。与团队分享经验，也从团队成员的实践中学习。别忘了继续探索文档。

如需从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可探索 [VS Code 练习][vscode]、[GitHub Copilot app 练习][app]或 [Copilot cloud agent 练习][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
