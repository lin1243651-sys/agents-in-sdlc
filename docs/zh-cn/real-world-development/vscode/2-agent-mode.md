---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员往往需要找到所有相关文件、逐一修改、运行测试、调试，再找出遗漏的文件并继续修改。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可在 IDE 中自主执行工作。它会先探索项目结构，进行必要修改，运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同的工作方式。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 智能体模式探索项目、找到相关文件并协调更改。
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现功能。
- 在将代码合并到项目之前，审查智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，希望用户能够按类别和发行商筛选。上一练习已添加 publishers helper，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

修改代码前，先查看网站当前功能。

网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准文档对象模型 (DOM) API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 命令，用于启动单一 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），再选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后可查看其详细信息，包括标题、说明、发行商和类别。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；需要时会在相应步骤说明。

网站初始版本可以正常运行，但还可以添加新功能。先查看待办事项。通过模板设置项目时，系统已自动创建议题；让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入步骤中，以尽量获得一致体验。不过，由于大语言模型 (LLM) 具有概率性，收到的建议可能与本工作坊示例不同，这是正常现象。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，查看议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为议题排序。

## 查看说明文件

让智能体生成代码前，先查看将用于提供上下文的说明文件。`.github/instructions/ui.instructions.md` 包含为网站添加功能时的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型说明的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这样可以复用说明，将复杂说明拆分成更易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行三类更改：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，测试必须通过后才能将更改合并到代码库。Copilot 智能体模式可以完成这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择模型 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件。它应会找到数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。根据任务和代码库复杂度，这可能需要几分钟。过程中，编辑器可能会显示 **Keep** 和 **Undo**；工作完成后，每项更改都会提供相应操作，无需在工作尚未完成时选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能有测试暂时失败。这没有关系；Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。你了解到如何：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让智能体模式探索项目、找到相关文件并协调更改。
- 在将代码合并到代码库之前审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 加分探索练习——实现分页

游戏列表增加后，需要支持分页。使用本练习学到的技能，让 Copilot 更新网站以实现分页。可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页实现方式，例如是否允许用户选择页面大小，或使用固定大小。
- 编写提示时提供所需指导。
- 通过补充上下文并请求修改与 GitHub Copilot 迭代，这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途和适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
