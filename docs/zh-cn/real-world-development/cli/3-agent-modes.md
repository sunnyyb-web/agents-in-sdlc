---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 从问题实现筛选功能，并在 Interactive 模式下检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。接下来的几课将演示典型的代理驱动开发流程：从问题着手实现功能、验证代码和行为，最后成功合并。

> [!NOTE]
> 后续功能工作会沿用同一对话和分支。实际工作中，不同文件类型的更改通常会拆分到不同分支或 PR；本练习为了聚焦核心概念而简化了流程。

本课将学习：

- 从 GitHub 问题启动新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办问题描述了该功能，但例如类别组合等细节仍需在编码前确定。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码代理引入开发流程，并不会改变软件开发的基本原则，反而让这些原则更加重要。大多数开发者的流程大致如下：

1. 从已提交的问题开始，明确需要完成的工作。
2. 制定实现计划。
3. 编写并检查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程，可以确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作；代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式并检查计划，然后再使用 Autopilot 实现。

## 从问题开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有问题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的问题。
9. 按 <kbd>C</kbd> 将问题添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的问题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你在交由 Copilot 实现功能或执行任务前，先确定实现方案。复杂任务尤其应该花时间规划。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示框下方的模式指示器显示 **Plan**。
2. 在上一步添加的问题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定合适的实现方式，过程中可能会询问筛选功能应如何运作。根据你的偏好回答即可。

3. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 浏览计划，并要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

计划确定并检查完毕后，就可以开始实现。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中类似名称的选项。
2. 确认提示框下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 检查并验证实现

像检查其他代码一样，在合并前检查生成的代码并运行网站，确保结果符合预期。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与问题及规划时作出的决定进行比较。
4. 检查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 试用新功能

代码看起来没问题，但运行效果如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合方式。
4. 确认重置和无结果时的行为符合问题描述及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并检查了一项功能。本课中，你：

- 从 GitHub 问题启动了新的 Copilot 对话。
- 在 Plan 模式下明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 检查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，你将通过自定义说明深入了解代码生成，确保生成结果遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
