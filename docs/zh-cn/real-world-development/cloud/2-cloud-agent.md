---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿拉取请求，供开发人员审查。这样可以将繁琐工作交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本课将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 了解清晰界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初负责创建网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队想改善游戏发现体验。他们希望每款游戏的详情页都显示同一类别中的其他游戏，方便用户继续浏览。目前不需要精致的实现，只需要能用于 UX 验收测试的版本。虽然这项工作目前受阻，但优先级高于它的议题还有其他任务。

这些都是容易被降低优先级的任务，很适合交给 Copilot 云智能体。它可以异步处理任务，让开发人员先处理其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度，设置环境并开始处理任务。你可以在会话进行中或完成后查看 Copilot 的操作。准备好审查建议的解决方案时，它会在拉取请求中提及你。

## 明确限定任务范围的重要性

GitHub Copilot 没有魔法，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能包含复杂环节，因此[为 Copilot 云智能体明确限定任务范围][cloud-agent-best-practices]非常重要。应将它视为 AI 结对程序员：分阶段工作、学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

之前的练习介绍了自定义指令及其作用。如果已经完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读查看本存储库包含的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将把 `Code lacks documentation` 议题分配给云智能体。留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头；这些要求来自上述指令文件。稍后的练习还会再次审查这些内容。

## 为 Copilot 云智能体设置开发环境

创建代码通常需要特定环境并运行一些设置脚本，以确保环境状态正常。将任务分配给以类似软件工程师方式工作的 Copilot 时也一样。

云智能体使用 [GitHub Actions][github-actions] 执行任务。你可以创建配置在 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]，在它开始工作前运行，以便安装所需的开发工具和依赖项。本实验已提前配置好该工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用经过迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
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

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器。它允许你在存储库的 **Actions** 选项卡中手动运行工作流，以便测试工作流是否成功，而不必等待 Copilot 触发。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目的文档不是过时，就是完全缺失。这类技术债务常常无人处理，降低工作效率，也让维护代码库或帮助新开发人员加入团队变得更困难。Copilot 很擅长生成文档，因此非常适合处理这类议题。它会在后台生成所需文档，后续练习会审查它的成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 生成的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办事项列表和即将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定合适的计划，以及编写代码。

这通常需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来，我们会利用这一点：先再分配一个任务给云智能体，然后编写代码为应用添加功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优势是可以拆分工作：你处理一组任务时，它可以处理另一组。即使为设计团队添加相关游戏区域不需要很长时间，也可以将这段时间用于其他工作。现在就把任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
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

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二个请求。Copilot 云智能体的工作方式类似软件工程师，因此你不需要主动监控，而是在工作完成后进行审查。接下来，我们将了解如何创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码从一种框架迁移到另一种框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保安全地执行操作；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用我们在前面的模块中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
