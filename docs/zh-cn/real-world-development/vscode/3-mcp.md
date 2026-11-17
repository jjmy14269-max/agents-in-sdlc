---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应先确认该功能在浏览器中正常运行。无需亲自逐一操作应用，你可以连接 **Playwright MCP 服务器**，让 Copilot 操控真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍：

- 什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何通过新工具扩展 Copilot；
- 如何将 Playwright MCP 服务器添加到工作区；
- 如何让 Copilot 使用它在浏览器中手动测试筛选功能；
- 如何发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式能够访问编辑器以外的工具时，功能会大幅增强。Copilot 使用 Model Context Protocol (MCP) 与这些工具交互，这是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理及其与上下文、LLM 和工具的交互示意图，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问外部工具和服务，从而利用资源获取最新信息，并使用工具代为执行操作。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信；外部工具可以是现有 API，也可以是 NPM 包等本地工具。每个 MCP 服务器都提供一组 AI 智能体可以访问的工具和资源。

下面介绍两个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化功能，让 AI 智能体浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以访问不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助用户发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用 MCP 服务器前，请仔细审查其源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

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

项目模板预先配置了 `github` 条目，因此 Copilot 能在上一练习中读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 通过 HTTP 连接到该服务器，并在 Copilot 首次使用其工具时要求你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 控制浏览器，正好用于测试新功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

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

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动服务器。`--headless` 标志会让 Playwright 在后台运行浏览器，不显示可见窗口；由于 codespace 没有桌面环境，因此必须使用此设置。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要服务器提供的工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要这些工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 操控真实浏览器。无需亲自操作应用来检查更改，智能体可以打开页面、导航、应用筛选条件、读取结果并汇报观察结果。整个过程都在对话中完成，可快速确认功能是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会根据结构化的已标记元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同。这意味着简单的功能检查也可以顺便快速确认无障碍情况。

连接服务器并启动应用后，提示 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成测试，并报告结果。根据议题中的验收标准审查其报告；如果发现问题，可以继续提问，或让 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器。如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），让团队进行审查。首先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，例如使用 github.com 或 GitHub 命令行界面（CLI）。既然已经在使用 GitHub Copilot，不妨让它代为创建 PR。它可以找到相关议题并将 PR 与议题关联起来。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉菜单中已选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你通过 GitHub 身份验证；按照提示授权即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结和下一步

恭喜！本练习中你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何通过新工具扩展 Copilot；
- 将 Playwright MCP 服务器添加到工作区；
- 使用服务器在发布功能前通过浏览器手动测试；
- 发布分支并为筛选功能创建拉取请求。

你刚刚使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。下一步将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
