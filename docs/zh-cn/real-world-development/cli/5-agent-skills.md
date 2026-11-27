---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已经手动验证代码能否运行，并使用说明文件确保代码符合标准。那测试、lint 和其他持续集成（CI）任务该怎么办？

这类任务很适合使用**Agent Skills**。技能可以帮助 Copilot 正确执行这些操作。

本课将学习：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

Agent Skills 将可复用的任务说明、可执行脚本和辅助资源打包在一起，由代理按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 `SKILL.md` Markdown 文件。该文件使用 frontmatter 中的 `name` 和 `description` 定义技能用途，并概述技能功能及调用时机。文件夹还可以包含供技能调用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户的技能文件夹 `~/.copilot/skills` 中。

## 探索技能

查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`，了解描述如何帮助 Copilot 判断何时调用技能。
3. 阅读说明，了解它如何指导 Copilot 执行测试和 lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

Copilot CLI 支持直接调用技能，也可以通过自然语言调用。让 Copilot 运行技能中的三项检查：

1. 返回筛选功能的 Interactive 模式对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后显示的报告。

## 自定义报告

现在改进报告，使其说明运行了哪些检查、每项是否成功，以及工具实际报告的结果。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用之前的同一提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后显示的报告，并与第一次的报告比较。

## 总结和后续步骤

你自定义并使用了现有的 Agent Skill。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的格式。
- 重新加载并运行了技能。

该更改会与筛选功能一起包含在功能 PR 中。接下来，通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
