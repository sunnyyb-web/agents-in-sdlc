---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。任务完成后，代理会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给代理处理，加快开发流程，让开发者专注于更重要的工作。

本课将探索 Copilot cloud agent 如何：

- 自定义代码生成环境。
- 安全地执行操作。
- 处理范围明确的问题。
- 接受分配的问题。

## 场景

Tailspin Toys 有一些技术债务。最初创建网站的承包商没有为项目编写文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能：在每款游戏的详情页上显示同类别的其他游戏，以便访问者继续浏览。团队目前只需要用于验收测试用户体验的初步实现。这项工作虽然尚未完成，但优先级低于其他问题。

这两项任务都可能被不断推迟，适合交由 Copilot cloud agent 异步处理。开发者可以继续处理其他工作，之后再回来审查代理的结果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。你可以通过多种方式启动任务，例如[将 GitHub 问题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。代理收到任务后，会创建草稿拉取请求跟踪进度、设置环境并开始工作。你可以在工作进行中或完成后查看 Copilot 会话。解决方案准备好供审查时，代理会在 PR 中通知你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，也可能包含许多复杂细节。因此，请[仔细界定交给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、学习、试验并不断调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明如何指导 Copilot。如果你已经完成相关练习，就会看到 `.github/copilot-instructions.md` 的作用；否则，可以先快速浏览。在分配任务前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

- `.github/copilot-instructions.md`：检查 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 问题分配给 cloud agent。审查 PR 时，留意 TSDoc 文档注释、TypeScript 约定和文件头注释；这些要求来自说明文件。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要配置环境并运行设置脚本。Copilot 也一样，它会像软件工程师一样完成任务。

Cloud agent 使用 [GitHub Actions][github-actions] 执行工作。你可以创建专用设置工作流 `.github/workflows/copilot-setup-steps.yml`，在代理开始前运行，以便安装所需工具和依赖项。项目已预先配置此工作流，以支持本练习。它会提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用的迁移和预置 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅设置步骤所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro 和 Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有几个要点：

- 它只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于在分配任务前验证工作流。

## 添加文档

许多项目缺少文档或文档已过时，这会拖慢工作并增加维护难度。Copilot 擅长生成文档，因此可以把这类问题交给 cloud agent，稍后再审查结果。

1. 在新的浏览器标签页中访问 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 问题分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 等待几分钟，直到 Copilot 创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办列表。这段时间内，Copilot 会创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方式。

13. 查看 Copilot 将要执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理问题的更新。
15. 选择 **View session**。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码并了解当前状态，如何暂停以制定合适的计划，以及如何创建代码。

整个过程可能需要几分钟。Copilot cloud agent 的主要优点之一是能够异步处理任务，让你专注于其他工作。接下来利用这一点：再分配一个任务，然后开始为应用添加功能。

## 在游戏详情页添加相关游戏

将工作拆分后，你可以专注于一项任务，同时让 Copilot cloud agent 处理另一项。现在将“添加相关游戏”分配给它：

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新问题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建问题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将 Copilot 分配给问题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 使用眼睛表情符号表示正在处理问题](../../../_images/ex4-issue-eyes-emoji.png)

稍后你会看到第一条问题评论中出现一组 👀，表示 Copilot 已开始处理第二个请求。Copilot cloud agent 的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。接下来，了解如何创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以把问题分配给 Copilot，让它异步处理技术债务、开发功能或迁移框架代码。

本课中，你：

- 自定义了代码生成环境。
- 了解了如何安全地执行操作。
- 学习了明确界定问题范围的重要性。
- 将问题分配给 Copilot。

现在 cloud agent 正在后台工作。接下来，我们将了解如何创建和使用自定义代理。Cloud agent 也可以使用 [MCP 服务器][cloud-agent-mcp]和自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 问题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
