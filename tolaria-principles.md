---
type: Note
related_to: "[[tolaria]]"
onboarding: 1
---
# 核心原则

Tolaria 建立在几条核心原则之上，它们指导着我们的设计决策：

### 📑 Files-first（文件优先）

你的笔记就是普通的 Markdown 文件。它们可移植、能被任何编辑器打开、不需要任何导出步骤。你的数据只属于你，不属于任何 App。

### 🔌 Git-first（Git 优先）

每个 vault 都是一个 Git 仓库。你拥有完整的版本历史，可以使用任意 Git 远程仓库，并且完全不依赖 Tolaria 服务器。

### 🛜 Offline-first, zero lock-in（离线优先，零锁定）

无需账号、无需订阅、不依赖云。你的 vault 完全可以离线工作，将来也始终如此。即使你不再使用 Tolaria，你什么都不会丢。

### 🔬 Open source（开源）

Tolaria 是免费且[开源](https://github.com/refactoringhq/tolaria)的。我最初是为我自己（[[luca-rossi]]）构建它，然后分享给更多人。

### 📋 Standards-based（基于标准）

笔记是带 YAML frontmatter 的 Markdown 文件。没有专有格式，没有锁定数据。如果有一天你决定离开 Tolaria，所有内容都能用标准工具直接打开。

### 🔍 Types as lenses — not schemas（类型是视角，而非约束）

Tolaria 中的类型是导航辅助，而不是强制约束。没有必填字段、没有校验，只是帮你分类、便于查找的友好标签。

### 🪄 AI-first — but not AI-only（AI 优先，但不仅限 AI）

基于文件的 vault 非常适合与 AI agent 协作，但你完全可以自由选择任何工具。我们目前支持 Claude Code 和 Codex CLI，你也可以用任何 AI 编辑这个 vault。我们提供 AGENTS 文件（[[agents]]）供你的 agent 参考。

### ⌨️ Keyboard-first（键盘优先）

Tolaria 为希望尽可能使用键盘的高级用户而设计。[[tolaria-editor]] 和 [[command-palette]] 的很多设计都基于这一原则。

### 💪 Built from real use（源于真实使用）

Tolaria 最初是为了管理我个人 10,000+ 条笔记的 vault 而创建的，我每天都在用它。每一个功能都源于一个真实的问题。
