---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些内容。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解如何完成。

本练习将学习：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会比较提示的更新前后效果。Copilot cloud agent 会异步处理 GitHub 问题，因此难以实时并排展示效果。稍后的练习中，你可以在审查 cloud agent 创建的 PR 时看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 界面应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

你将通过说明文件确保 Copilot 获得这些指导，并按要求完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。说明可以指定编码约定、库和注释风格，也可以作用于整个存储库或特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：针对该存储库的每个请求都会发送此文件。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建此类文件，例如 TypeScript、Astro、界面组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论说明文件的创建方式超出本练习范围。示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并说明代码应如何编写以及应避免哪些做法。

说明文件没有唯一的正确写法。通过实践找到适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。浏览本项目时，你会发现许多任务都有对应文件，例如[界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关练习会在适用时介绍具体操作。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

查看此存储库随附的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 指导，这些内容适用于 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置相对于存储库根目录的 glob；例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用这些说明。
5. 查看此项目中有关编写单元测试的具体说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。注意其中对其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以拆分较大的说明并提供示例。文件路径相对于说明文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍项目编码标准，但目前尚未要求添加代码内文档。接下来将添加 TSDoc 文档注释和文件头注释的规则。

## 添加新的存储库标准

本练习只有一项编辑任务：添加项目级规则，要求代码使用 TSDoc 文档注释和文件级注释。下一练习中，cloud agent 会根据这些说明处理问题。

编辑前先创建分支。cloud agent 会读取目标问题所在分支的说明；下一练习中，问题将以 `main` 为目标，因此你需要将本次更改合并到 `main`。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`，找到约在第 27 行的 **Code formatting requirements** 部分。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可在存储库级 `.github/copilot-instructions.md` 中添加全局指导，或在 `*.instructions.md` 文件中为特定语言、文件类型或任务提供指导。项目级标准适合放在存储库级文件中。

## 提交、推送并合并说明更新

cloud agent 会从目标问题所在分支读取说明。下一练习中，代理会从 `main` 创建分支，因此必须先将说明更改合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。

> [!TIP]
> 如果本练习中希望直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何获取项目说明中的上下文，并向 `.github/copilot-instructions.md` 添加了新的全局标准。后续练习中，你会看到 cloud agent 创建的 PR 如何体现这项标准。

接下来，将第一个问题分配给 Copilot cloud agent。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
