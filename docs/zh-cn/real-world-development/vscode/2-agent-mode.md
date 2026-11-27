---
title: "练习 2 - 使用 Copilot Agent Mode 添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏的文件并再次更新，过程可能不断重复。

Copilot Agent Mode 可以帮助完成这些工作。

Agent Mode 可以在 IDE 中更自主地工作，类似开发者：先探索项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。接下来将了解如何使用 Agent Mode 为网站添加新功能。

> [!NOTE]
> Agent mode 和 cloud agent 名称相似，但适用于不同场景。Agent mode 在 IDE 中执行任务，适合快速反馈和交互。Cloud agent 则像团队成员一样异步工作，通过问题和拉取请求与你协作。

本练习将学习：

- Copilot Agent Mode 如何探索项目、识别相关文件并协调更改。
- GitHub Copilot Agent Mode 如何跨界面和数据层实现功能。
- 合并前如何检查 Copilot Agent Mode 生成的更改和测试。

## 场景

游戏列表不断扩大，你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot Agent Mode 完成数据层、界面和测试的其余工作。

## 运行 Tailspin Toys 网站

修改前先了解 Tailspin Toys 网站当前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个单独的 Astro 应用，页面通过 frontmatter 中的 Drizzle ORM 直接查询本地 SQLite 数据库，并在构建时生成为静态 HTML。应用没有单独的后端 API 或客户端界面框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过范围明确的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下 URL 的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 内网站的私有隧道。

### 探索网站

网站的主要功能包括：

- **主页**：显示桌游列表、标题、图片和描述。
- **游戏详情页**：选择游戏后，会看到包含标题、描述、发行商和类别等信息的详情页。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每个步骤会在适用时说明。

网站的初始实现已可正常使用，但还需要添加更多功能。项目模板会自动创建一组待办问题。让 GitHub Copilot 列出这些问题：

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。该代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本练习作者并非推荐某个模型。编写本练习时使用的是 Claude Sonnet 4.5，因此将其列入说明，希望尽量保持体验一致。不过，大型语言模型具有概率性，收到的建议可能与本练习不同，这是正常现象。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，查看 GitHub 存储库中的待办问题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行列出所有问题的命令。
5. 检查生成的问题列表。

Copilot 还会根据它认为对用户最有用的程度为问题排序。

## 查看说明文件

让代理生成代码前，先查看用于提供工作上下文的说明文件。本练习将使用[用户界面（UI）说明](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含向网站添加功能的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括应用架构、组件设计/可测试性/无障碍原则，以及指向 Astro、Tailwind CSS 和 Drizzle 说明的链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用说明、拆分较大的说明，并为 Copilot 提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三类更新：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选界面。

合并前还需要运行并通过测试。Copilot Agent Mode 可以完成这些任务。

1. 可继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的代理下拉菜单中仍选中 **Agent**。
4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用与待办问题相关的要求提示 Copilot：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、界面和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。工作期间，编辑器中可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项；工作尚未完成时无需选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作期间可能会暂停，也可能出现测试失败。这是正常的。Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![完整的 Copilot Agent Mode Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 请始终检查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，你使用 GitHub Copilot Agent Mode 为 Tailspin Toys 网站添加了功能。你了解到：

- GitHub Copilot Agent Mode 可以跨界面和数据层实现新功能。
- Copilot Agent Mode 可以探索项目、识别相关文件并协调更改。
- 合并前应检查 Copilot Agent Mode 生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分练习 — 实现分页

随着游戏列表增加，需要支持分页。使用本练习中学到的技能，让 Copilot 为网站添加分页。考虑：

- 遵循现有最佳实践，包括使用说明文件。
- 确定分页方式，例如是否允许用户选择每页大小。
- 编写提示时提供足够指导，让 Copilot 按照你的预期实现功能。
- 可能需要多次迭代，向 Copilot 提出修改要求并提供上下文。这是正常的协作方式。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的 agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
