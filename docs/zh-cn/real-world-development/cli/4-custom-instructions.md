---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "查看存储库说明，添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅说明你想要*什么*代码，也描述代码应当*如何*组织。筛选功能已完成，现在查看 Copilot 使用的说明，添加一项文档标准，并将其应用到代码中。

在本课中，你将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TypeScript 文档注释 (TSDoc) 或 JavaScript 文档注释 (JSDoc) 说明用途、参数和返回值；如有可注入的 `db` 参数，也应予以说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

使用说明文件可让 Copilot 获取正确的信息，并按这些实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库，以及希望添加的注释类型。可以为整个存储库或特定文件类型创建说明，为任务提供上下文。

说明文件有两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个说明文件。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 不同环境对其他说明格式的支持各不相同。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

示例项目已包含一组说明文件。先查看现有文件，再进行更改，以便了解更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 确定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和项目现有文件的引用。这种方式可以将大型说明拆分为可复用的小文件，并为 Copilot 提供代码示例。

## 更新说明文件以符合团队指导

现有文件是不错的起点，但仍缺少一项指导。修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时载入存储库说明。保存说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时载入新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。确认新增的 TSDoc 注释准确说明代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新的标准应用于功能。具体来说，你：

- 了解了存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
