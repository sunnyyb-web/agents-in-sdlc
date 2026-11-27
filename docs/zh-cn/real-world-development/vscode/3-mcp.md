---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：Agent mode][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用 agent mode 构建了筛选功能。创建拉取请求前，应在浏览器中确认功能确实正常。与其手动操作应用，不如连接 **Playwright MCP 服务器**，让 Copilot 控制真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

当 agent mode 能访问编辑器以外的工具时，它会更加强大。Copilot 通过 Model Context Protocol (MCP) 连接外部工具和服务；它是一种代理与工具交互的标准方式。

![agent mode 的工作原理示意图，展示它如何与上下文、LLM 和工具交互，包括 MCP 服务器及 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 代理可以与外部工具和服务通信。代理可以通过 MCP 实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

MCP 服务器充当代理与外部工具和服务之间的桥梁，负责管理代理与现有 API 或本地工具（例如 NPM 包）的通信。每个 MCP 服务器都提供一组代理可以访问的工具和资源。常用服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，使代理能够创建和更新存储库、管理问题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，使代理能够浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细检查源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置当前 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板包含此 `github` 配置，因此 Copilot 能在上一练习中读取待办问题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用工具时需要使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 启动它。`--headless` 参数让 Playwright 在没有可见窗口的情况下运行浏览器，这适用于无法显示桌面的 Codespace。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. `.vscode/mcp.json` 中会在 `playwright` 配置上方显示 **Start** 操作。选择它以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步 Copilot 首次需要服务器工具时启动服务器，并询问是否信任。

## 测试筛选功能

连接服务器并启动应用后，让 Copilot 操作浏览器。它可以打开页面、应用筛选、读取结果并总结情况，你无需离开对话即可快速验证功能。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是截图。这意味着代理会读取结构化、有标签的元素（按钮、链接和列表项），与辅助技术采用的方式类似。因此，快速功能检查也可以顺便进行初步无障碍检查。

使用以下提示，让 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步执行操作并报告结果。将报告与问题中的验收标准进行比较。如果发现异常，请继续询问或要求 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果提示缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以发布 `filtering-vscode` 分支，并创建拉取请求供团队审查。

1. 在 Codespace 中打开 **Source Control** 面板，查看 Copilot 所做更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示 Copilot 更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。也可以让 Copilot 查找相关问题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认代理下拉菜单中选中 **Agent**，让 Copilot 可以使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证。
5. 确认 Copilot 搜索问题、找到正确问题并创建了 PR。
6. 选择 Copilot 提供的链接检查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 在发布前使用它通过浏览器手动测试了筛选功能。
- 发布分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 也可以用其他方式扩展 Copilot。接下来，创建[自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
