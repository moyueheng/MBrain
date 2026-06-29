---
type: Note
related_to: "[[tolaria]]"
onboarding: 2
---
# Sidebar（侧边栏）

Sidebar 是你的主导航工具。它以多种方式对笔记进行分组，帮助你快速找到所需内容。

### 📥 Inbox（收件箱）

尚未被 *整理* 的笔记。

这一设计受 [Tiago Forte](https://www.buildingasecondbrain.com/) 关于"捕获与整理分离"的理念启发。所谓"已整理"的笔记，是指你已经清楚它该归属何处：可能属于一个项目、一项职责、一个主题，或任何与你工作组织方式相关的概念。

建议每周回顾一次 Inbox，避免堆积过多。如果你不喜欢这种工作流，可以在设置（`cmd+,`）中关闭 Inbox。

你可以点击面包屑栏中的"圆圈任务"图标，或使用 `cmd+e` 把笔记标记为 *已整理*。

### 🗃️ All Notes（全部笔记）

顾名思义，整个 vault 中的每一条笔记。

### 📦 Archive（归档）

为那些你不想删除、又不想经常看到的笔记准备的永久归宿。大胆归档，保持 vault 整洁。归档的笔记仍会出现在搜索结果中，但不会出现在 sidebar 各区段里。

### ⭐ Favorites（收藏）

你手动置顶、希望常驻视野的笔记。适合用于活跃项目、日志条目或频繁访问的参考笔记。

你可以在 sidebar 中拖动来调整它们的顺序。

通过面包屑栏中的 ⭐ 按钮，或使用 `cmd+d` 即可切换收藏状态。

### 🔍 Views（视图）

你可以创建自定义视图，用复杂、嵌套的条件过滤笔记。视图编辑器会抓取所有可用属性，还支持一些进阶玩法（如正则表达式和自然语言日期）。

![CleanShot 2026-04-16 at 22.02.51@2x.png](attachments/1776369786040-CleanShot_2026-04-16_at_22.02.51_2x.png)

Views 本质上是简单的 YAML 文件，存放在 views 文件夹中（例如 [[views/active-projects.yml]]）。它们易于编辑，甚至可以由 AI 直接创建 👇

```yaml
name: Active Projects
icon: 🚀
filters:
  all:
  - field: type
    op: equals
    value: Project
  - field: Status
    op: equals
    value: Active
  - any:
    - field: status
      op: equals
      value: active
    - field: date
      op: after
      value: in 1 week
```

### 🧩 Types（类型）

Tolaria 中最主要的组织手段。每条笔记都有一个类型，默认是 [[note]]。

每种类型都可以有自己的图标和颜色。你可以在 sidebar 中右键点击它来修改，也可以直接编辑该类型文件的属性或 frontmatter。

Types 就是以 Markdown 文件的形式存储，例如 [[topic]] 或 [[project]]。

在 sidebar 中点击任意类型，即可在笔记列表中查看该类型的所有笔记。

### 📂 Folders（文件夹）

Tolaria 默认把笔记存放在 vault 根目录，但也会扫描 vault 的文件夹结构，你可以在 sidebar 底部导航它们。

Folders 被排在最后，因为它们是次要的组织方式，而不是主要方式。

### 🗑️ Trash（回收站）

没有回收站。当你删除一条笔记时，它就没了 —— 但 Git 历史是你的安全网。必要时，你总能从 Git 中恢复已删除的笔记。
