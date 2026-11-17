---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种关键方式是使用[指令文件][instruction-files]来指导代码生成。

本练习将介绍：

- 如何通过存储库自定义指令和按路径限定范围的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot；
- 如何在现有指令生效时生成筛选功能的第一个数据切片（publisher 辅助函数）；
- 如何在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 如何重新运行相同提示，观察生成的代码如何应用新标准；
- 如何将指令更新和筛选功能切片提交到 `main`，供下一练习使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有标准。Copilot 的结果具有非确定性。本练习旨在观察更新指令前后行为的*趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以让 Copilot 获取正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它了解你的编码风格和要求。你可以指定编码规范、库以及注释风格等，从而获得更贴合需求的建议和代码片段。可以为整个存储库创建指令，也可以针对特定文件类型创建指令，为任务提供上下文。

指令文件分为两种：

- `.github/copilot-instructions.md`：每次针对该存储库提出请求时，都会将这个指令文件发送给 Copilot。文件应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建这些文件。例如，为 TypeScript 或 Astro 编写指南，或说明如何创建 UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中工作时，指令文件仅用于 Copilot Chat 的代码生成，不适用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 也[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不深入介绍如何创建指令文件，但示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 中的指令限定为项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例说明应该和不应该采用的写法。

指令文件没有唯一的正确写法，AI 的使用方式也不止一种。通过实践可以找到适合项目和场景的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。浏览本项目中的文件时，你会看到其中包含多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同（例如在 VS Code 中选择 **Configure Chat → Generate Agent Instructions**，或在 Copilot CLI 中运行 `/init`）。当前产品的课程会在相关步骤中介绍具体方法。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义指令文件

花些时间查看本存储库自带的指令文件：其中有一个核心文件 `copilot-instructions.md`，以及一系列用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看 `applyTo` 字段，它通过相对于存储库根目录的 glob 模式，指定指令适用于哪些文件。此处匹配任何 TypeScript 测试文件（例如符合 `**/*.test.ts` 的文件）。
5. 查看此文件中有关创建单元测试的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令拆分为较小、可重用的文件，并提供可供 Copilot 参考的示例。（这些路径是相对于指令文件的，而不是相对于存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求添加代码文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习中打开的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新的终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始新的对话。模式和模型选择因练习而异；每个步骤会在需要时说明。

要查看自定义指令的影响，先在当前指令文件生效时发送提示。稍后更新指令后，再发送相同提示进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应存放的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意建议的辅助函数是带类型的函数：第一个参数为 `db` 客户端，返回类型化的 publisher 数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层规范（该文件适用于 `src/lib/*.ts`）。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察更新指令后行为是否更加一致。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来更新项目编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应在第 27 行附近）。此处记录了项目编码标准，但尚未规定代码文档要求，因此 Copilot 生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指令。刚添加的文档注释规则属于项目级标准，因此放在存储库级文件中最合适。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看建议的文件是否以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看建议的函数是否包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中。下一练习会在此基础上继续实现功能。

## 提交指令并推送分支

指令文件和其他资源一样，使用常规源代码管理流程进行管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

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

分支推送后，创建拉取请求并将其关联到待办列表中的文档标准议题。也可以让 Copilot 查找议题、创建用于关闭议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已连接 GitHub 工具，无需另行设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的 publisher 辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 提示你允许它代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，提示 Copilot 将其合并到 main，并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示你允许它执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和下一步

你了解了 Copilot 如何从指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下工作：

- 发送代码生成提示，观察现有指令对 Copilot 输出的影响；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新运行相同提示，观察建议的代码如何应用新标准；
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
