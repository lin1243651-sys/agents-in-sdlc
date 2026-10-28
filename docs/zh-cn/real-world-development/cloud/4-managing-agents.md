---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽然异步自主工作，但监控任务仍然很重要。

GitHub 提供了多种云智能体任务管理工具，包括 [agents 页面][agents-page]。在这个任务控制中心，可以查看所有创建了未关闭拉取请求 (PR) 的智能体任务、查看它们执行的操作，并在会话进行中引导工作。

在本课中，你将：

- 浏览 agents 页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将高对比度模式任务分配给智能体后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，将这项功能一并纳入很合理。现在引导智能体同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 工作状态和采用的方案而变化。

4. 查看右侧显示的 PR 面板，以监控 PR 和已更改的文件。

## 引导云智能体

现在让 Copilot 在实现高对比度模式时，也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents 页面中的云智能体任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认提示并将其纳入任务流程。

## 让 Copilot 继续工作

Copilot 会继续处理更新后的任务。完成发送提示时正在执行的步骤后，它会将新请求加入工作流程。

这可能还需要几分钟。此时可以暂停一下，回顾迄今学到和探索的内容。

## 总结和后续步骤

本课探索了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。在这里可以跟踪所有进行中和已完成的任务、审查工作，并重定向正在运行的任务以调整范围或补充指导。

你探索了以下内容：

- 使用任务控制中心和 agents 页面监控云智能体任务。
- 重定向正在进行的会话，要求添加额外功能。

Copilot 正在完成无障碍功能，接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
