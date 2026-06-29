---
type: Note
related_to: "[[tolaria]]"
onboarding: 6
---
# 底部栏

底部栏是 Tolaria 的状态栏——以信息展示为主，交互为辅。它让你随时了解当前状态，又不会打扰你的工作。

### 📂 Vault 选择

点击左下角的 vault 名称可以切换当前 vault、打开新的本地文件夹、通过克隆 Git 仓库创建新 vault，或者管理哪些 vault 被纳入你的图谱。

![CleanShot 2026-04-17 at 15.35.16@2x.png](attachments/1776432929539-CleanShot_2026-04-17_at_15.35.16_2x.png)

当在 Settings → Vaults 中启用了 `Use multiple vaults at the same time` 后，此菜单会显示复选框，供你选择 Tolaria 要同时加载的 vault。被纳入的 vault 会出现在搜索、笔记列表、快速打开、反向链接和 wikilink 导航中。当前选中的 vault 仍然控制仓库相关的操作，例如提交、同步、保存的视图和文件夹导航。

### 🔄 Git 同步状态

显示 vault 中未提交的变更数量。当你看到一个数字时，说明有未提交的编辑。点击可以查看具体变更，或直接点击 `commit` 进行提交。

如果你启用了 [[autogit]]，提交按钮会自动生成提交信息，并将所有变更推送到仓库的主远程分支。

### ⚡ History

轻量级的 git 历史视图，展示所有笔记的最近变更。可以查看笔记何时被修改、由谁修改，并快速访问变更详情。

![CleanShot 2026-04-17 at 15.32.38@2x.png](attachments/1776432798617-CleanShot_2026-04-17_at_15.32.38_2x.png)

### 🧮 App 版本

显示当前 Tolaria 的版本号。在调试或检查新功能时很有用。

### 🪄 AI 状态

显示哪些 AI Agent 可用——目前支持 Claude Code 和 Codex——以及当前选中的是哪一个。你也可以在这里切换 AI agent。

![CleanShot 2026-04-17 at 15.51.39@2x.png](attachments/1776433913782-CleanShot_2026-04-17_at_15.51.39_2x.png)

### 📣 反馈

你可以前往 GitHub issues 提交 bug 报告或功能请求。
