---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还需要处理数据、外部服务，并允许 Copilot 使用更多自动化工具。MCP 服务器可以让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

本课将学习：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让代理驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证界面更新还需要实际与界面交互。你将允许 Copilot 像用户一样访问正在开发的网站，以自动检查更改并增强对功能行为的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理提供了与外部工具和服务通信的方式。通过 MCP，代理可以实时访问最新信息，并代表你执行操作。

MCP 服务器负责连接 AI 代理与外部工具和服务。每个 MCP 服务器都提供一组代理可以访问的工具和资源。常用服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、问题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 请像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并评估安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器的对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

此前你已经手动确认功能按预期运行。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自动找到 MCP 服务器，不一定需要明确要求使用它。不过，指出要使用的服务器有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认所有功能按预期运行。

Copilot 会启动服务器，使用 Playwright 与网站交互，停止服务器，并提供报告。

## 总结和后续步骤

恭喜！你通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中探索了功能。回顾一下，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让代理驱动浏览器并探索筛选功能。

接下来，你将[创建 QA 自定义代理][next-lesson]，把技能和浏览器工具结合起来，形成专门的工作流程。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
