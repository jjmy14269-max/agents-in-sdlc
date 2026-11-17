---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你让 Copilot 云智能体执行了三项任务，旨在改善用户体验并添加功能。云智能体虽然以异步、自主的方式工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求（PR）的智能体任务，了解已执行的操作，甚至可以引导正在进行的会话。

本课将介绍：

- 如何使用智能体页面监控云智能体任务；
- 如何引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体来创建高对比度模式后，团队意识到也应该添加浅色模式。既然正在更新网站样式并添加切换功能，不妨一并实现该功能。现在需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
    - 更新代码文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。这些操作可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成任务所需的各项步骤。

> [!NOTE]
> 具体列出的步骤会根据 Copilot 的工作进度和采用的方法而变化。

4. 还要查看右侧显示的拉取请求（PR）窗格。可以在其中查看 PR 和已更改的文件，以便进一步监控任务。

## 引导云智能体

了解当前正在执行的任务后，接下来请求 Copilot 在实现高对比度模式的同时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 采用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到会话显示已完成设置并开始工作。此时，你会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！发送消息时，它正在执行的步骤完成后，就会把新请求纳入工作流程。

这通常需要几分钟。你可以稍作停顿，回顾目前学到和探索的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。借助这个任务控制中心，可以跟踪所有活动和已完成的任务、审查正在执行的工作，还可以重新指引正在进行的任务来调整范围或提供补充指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重新指引正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
