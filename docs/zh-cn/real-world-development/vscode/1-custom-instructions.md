---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Agent mode →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能访问这些内容。你可以通过多种方式向 Copilot 提供上下文，其中一种关键方式是使用[说明文件][instruction-files]指导代码生成。

本练习将学习：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 在当前说明文件的指导下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次发送相同提示，观察生成代码如何应用新标准。
- 将说明更新和筛选功能代码提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不符合所有自定义标准，因为 Copilot 的输出具有概率性。本练习的重点是观察更新说明后行为的变化趋势，而不是让输出逐字匹配示例。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 界面应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

你将通过说明文件确保 Copilot 获取这些指导，并按要求完成任务。

### 自定义说明

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。说明可以指定编码约定、库和注释风格，也可以作用于整个存储库或特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：针对该存储库的每个请求都会发送此文件。它应包含适用于大多数请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建此类文件，例如 TypeScript、Astro、界面组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也支持其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

示例项目展示了一种有代表性的说明文件组织方法：

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

## 为更改创建分支

按照最佳实践，为本次更改创建分支：

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每个步骤会在适用时说明。

先在当前说明文件的指导下发送提示，之后更新说明并再次发送相同提示，比较结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数的存放位置。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 可以修改文件。

   ![Chat 视图中的代理选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，可能会修改辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，接受 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意生成代码**缺少** TSDoc 文档注释和文件头注释。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习重点仍是观察添加说明后行为的一致性改进。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于提供项目级指导。现在为存储库添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到约在第 27 行的 **Code formatting requirements** 部分。它说明了项目编码标准，但尚未规定代码内文档要求。
3. 在现有标准下方添加以下内容，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 存储库级 `.github/copilot-instructions.md` 适合放置全局标准；`*.instructions.md` 文件则适用于特定语言、文件类型或任务。

## 再次发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除当前对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已经引导 Copilot 遵循新的项目标准，并将标准应用到下一练习会继续使用的代码中。

## 提交并推送说明文件

说明文件与存储库中的其他资产一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新和 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办列表中的文档标准问题。Copilot 可以使用已连接到项目的 GitHub 工具帮你完成此操作。

> [!NOTE]
> 这是本练习中首次让 Copilot 代表你操作 GitHub。项目模板已经为工作区连接 GitHub 工具，因此无需进行额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保代理选择器中选中 **Agent**。
3. 让 Copilot 查找说明文件更新问题，并从当前分支创建 PR，说明其中包含说明更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找问题并创建 PR。
5. 系统提示 Copilot 代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 系统提示 Copilot 执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你了解了 Copilot 如何获取说明文件中的上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有说明的影响。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 再次发送相同提示，观察代码如何应用新标准。
- 将说明更新和发行商辅助函数基础提交到 `main`，供下一练习使用。

接下来，在 [agent mode][next-lesson] 中让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
