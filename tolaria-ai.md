---
type: Note
related_to: "[[tolaria]]"
onboarding: 7
---
# Tolaria 中的 AI

Tolaria 拥抱 AI，但并不把它作为强制要求。你可以根据自己的工作流选择不同方式。

### AI 无需集成即可使用

因为 Tolaria 是基于文件的 vault，并且有 [[agents]] 文档说明其格式，任何 AI CLI 都能与它良好协作：Claude Code、Codex、OpenClaw，或你自己的助手 —— 都能理解 vault 的结构。

把 Tolaria 笔记当作普通的代码库，自然地提示即可。

不过，Tolaria 也提供可选的内置 AI 集成，会调用你已经安装的 CLI 助手：

### Quick Prompt Mode（快速提问模式）

按 Cmd+K，然后输入一个空格，即可在命令面板中进入 Quick Prompt Mode。添加 wikilinks 来传入笔记上下文。适合一次性提问和临时任务。

![CleanShot 2026-04-17 at 15.39.34@2x.png](./attachments/1776433419379-CleanShot_2026-04-17_at_15.39.34_2x.png)

你可以输入 [[ 并从自动补全中选择，引用已有的笔记。

### AI Panel（AI 面板）

点击面包屑栏中的 AI 图标可打开完整的 AI 面板，它能保留与 AI agent 的持续对话历史。

![CleanShot 2026-04-17 at 17.52.52@2x.png](./attachments/1776441193704-CleanShot_2026-04-17_at_17.52.52_2x.png)

### 连接状态

底部栏会显示当前可用的 AI agent 连接。目前你可以在 Claude Code 和 Codex 之间切换。


