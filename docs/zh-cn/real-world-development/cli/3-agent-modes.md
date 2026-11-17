---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一项小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作方式，帮助团队以正确的方式构建正确的功能。本课是多节课程中的第一节，将按照典型的智能体驱动开发流程展开：从议题生成新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作会继续使用同一对话和分支。通常会为不同类型的文件创建不同分支或 PR，但本研讨会会采用简化方式，帮助你专注于核心概念。

本课将介绍：

- 如何从 GitHub 议题开始新的 Copilot 对话；
- 如何在 Plan 模式下定义要求；
- 如何使用 Autopilot 模式实现新功能；
- 如何审查代码；
- 如何在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和发行商筛选游戏。待办列表中的议题描述了该功能，但筛选类别组合等细节需要在编码前达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 在限定范围内实现功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则。事实上，这些原则变得更加重要。大多数开发人员的工作流程大致如下：

1. 从描述待完成工作的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查完成且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循标准流程可以确保 AI 生成的代码满足既定要求，并像手动编写的代码一样经过审查。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并反复迭代，直至完成任务。

先使用 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动后再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

此时会看到提示以 `#7`（或类似编号）开头。`#` 用于将 GitHub 议题或拉取请求（PR）添加到上下文。

## 规划筛选功能

规划可以先确定实现功能或执行任务的方式，再交给 Copilot。处理复杂任务时，最好先花些时间规划。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能的工作方式。根据偏好回答，没有错误答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 向上和向下滚动，审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已经完成并审查，接下来开始实现。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据计划反复迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

生成代码后，和其他代码一样，必须在合并前审查。现在审查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与议题要求以及规划时做出的决定进行比较。
4. 审查代码后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查输出，并要求 Copilot 解决失败项。

## 探索新功能

代码看起来不错，但能否运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的筛选组合。
4. 确认重置和无结果时的行为符合议题要求和之前的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话；
- 在 Plan 模式下定义要求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

接下来[使用自定义指令][next-lesson]，深入了解代码生成方式并确保代码遵循书面规范。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
