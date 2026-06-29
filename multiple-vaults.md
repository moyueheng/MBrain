---
type: Note
related_to: "[[tolaria]]"
onboarding: 6.5
---
# 多 Vault

Tolaria 可以同时管理多个 vault。当你希望保持独立的文件夹或 Git 仓库，同时又想把它们作为一个统一的知识图谱来搜索和导航时，这个功能非常有用。

开启步骤：

1. 打开 Settings。
2. 进入 Vaults。
3. 启用 `Use multiple vaults at the same time`。
4. 打开左下角的 vault 菜单。
5. 勾选你想要纳入统一图谱的 vault。

被纳入的 vault 会同时出现在笔记列表、搜索、快速打开、反向链接和 wikilink 导航中。当某个笔记可能存在歧义时，Tolaria 会显示一个小的 vault 标签，让你知道它属于哪里。

每个 vault 仍然保留各自的文件和 Git 仓库。提交、同步、保存的视图、文件夹导航以及 vault 修复操作仍然归属于当前选中的 vault。使用 `Manage vaults` 可以重命名 vault、选择颜色，并设置新建笔记的默认 vault。

当 Tolaria 需要跨 vault 的 wikilink 时，会使用目标 vault 的稳定别名，例如 `[[team/projects/alpha]]`。同一个 vault 内部的链接则保持简洁。
