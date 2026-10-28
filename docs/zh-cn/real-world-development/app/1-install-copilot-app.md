---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和持续集成 (CI) 管道可直接使用。它适用于同时指挥多个智能体的工作流：每个智能体都在隔离的工作区中运行，并可自动执行重复任务。Node.js 已安装、项目副本也已准备就绪，现在安装应用并连接存储库。

在本课中，你将：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 尝试快速聊天，了解应用本身。

## 场景

团队正采用 AI 智能体处理不断增长的待办事项。Copilot app 提供统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将完成安装和连接，并熟悉如何围绕项目发起对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。使用 Copilot Business 或 Enterprise 时，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。

## 安装并配置 GitHub Copilot app

Copilot app 支持 Windows、macOS 和 Linux。接下来安装应用、完成身份验证，并将 Tailspin Toys 存储库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 下载页面][download-app]。
2. 下载适用于当前平台的应用，并按下载页面上的说明安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按提示完成身份验证。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果新手引导流程提供存储库选择，请选择从模板创建的 Tailspin Toys 存储库。也可以跳过存储库选择，并在完成引导后添加项目。
6. 按提示选择主题，然后完成新手引导。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已经列出，无需再次添加。
8. 如果存储库尚未连接，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和存储库名称。
9. 确认侧边栏中显示了存储库。此处使用的布局会将 `tailspin-toys` 等存储库文件夹归在 **Sessions** 下，并在每个存储库下显示会话。

## 熟悉工作区

项目连接后，先了解工作区布局。应用在侧边栏中分为几个区域：

- **New**：在这里启动与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成显示拉取请求。在这里可以浏览和筛选拉取请求、查看 CI 状态并进行审查，无需离开应用。
- **Issues**：以同样的方式显示议题。在这里可以浏览和筛选议题，并从议题启动会话。
- **Automations**：按计划或按需运行的已保存智能体任务，适合管理待办事项、定期项目维护或其他重复工作。总结课程会将其作为后续方向，而不是新增一项工作坊练习。
- **Customize**：通过模型上下文协议 (MCP) 服务器、插件、技能和其他组件扩展 Copilot app。后续会在这里配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需创建分支或工作区。首次聊天后会显示此区域。如果尚未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：显示存储库文件夹及其智能体会话。选择 `tailspin-toys` 等存储库名称可打开存储库视图；选择存储库下的会话可返回该对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话而不会发生更改冲突。添加星级评分时会启动第一个会话。

完成工作坊的过程中，你会逐步了解这些工作区功能。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找模板创建的待办事项

Tailspin Toys 与大多数项目一样有待办事项。现在查看通过模板创建项目时生成的议题。

1. 在侧边栏的 **Sessions** 下选择 Tailspin Toys 存储库文件夹，而不是其下方的单个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题阅读详情。每个议题也可以作为智能体会话的启动点。完成第一次小型更改后，将从筛选议题启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示添加到 Copilot app 的存储库中的工作项。若要查看其他存储库中的工作项，请将那些存储库添加到应用。

## 尝试快速聊天

快速了解应用的一种方式是用它来了解*应用本身*。**快速聊天**适合无需创建分支或工作树即可提问或集思广益，因此适合快速的一次性问题，无需创建会话。

1. 在侧边栏中，选择 **Chats** 旁的 **+** 打开新聊天。如果尚未显示 **Chats**，选择 **Sessions** 旁的 **+** 开始第一次聊天；之后会显示 **Chats**。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体而不发生更改冲突。你可以随时继续对话或开始新聊天。

## 总结和后续步骤

你已安装 GitHub Copilot app、连接项目并探索工作区。你学习了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中找到模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
