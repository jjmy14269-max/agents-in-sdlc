---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应该怎么做。

本练习将介绍：

- 如何通过存储库自定义指令和按路径限定范围的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot；
- 如何在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，这里不会运行前后对比提示。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到指令文件的作用。

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
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它通过相对于存储库根目录的 glob 模式，指定指令适用于哪些文件。此处匹配任何 TypeScript 测试文件（例如符合 `**/*.test.ts` 的文件）。
5. 查看此文件中有关创建单元测试的具体指令。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令拆分为较小、可重用的文件，并提供可供 Copilot 参考的示例。（这些路径是相对于指令文件的，而不是相对于存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但尚未要求添加代码文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来要在这里进行唯一一次编辑：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释头为代码添加说明。之后，云智能体会在本系列练习的后续步骤中处理你分配的议题时应用这条规则。

编辑前，请先创建一个分支（云智能体会从议题目标分支读取指令，不过你仍应妥善提交自己的更改）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习中将把议题分配给 Copilot，而 Copilot 会从 `main` 分支创建分支。因此，必须先将指令更新合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。下一练习中，云智能体就会使用这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接将更改提交到 `main`。此处采用分支，是为了让研讨会更贴近真实项目的工作方式。

## 总结和下一步

你了解了 Copilot 如何从指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来几节中，云智能体创建的拉取请求会展示这项标准的实际效果。

下一步，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
