---
type: Note
_organized: true
---
# AGENTS.md — Tolaria Vault

这是一个 [Tolaria](https://github.com/refactoringhq/tolaria) vault：由带 YAML frontmatter 的 Markdown 文件组成的文件夹，构成个人知识图谱。

请保持编辑内容与 Tolaria 当前的约定兼容。优先采用小幅的、人类可读的改动，而非大规模重构。

## 核心约定

- 每个文件一个 Markdown 笔记。
- 正文中的第一个 H1 是首选的显示标题。
- 当笔记没有 H1 时，旧版的 `title:` frontmatter 仍会作为回退读取。除非你在维护旧文件，否则不要将其添加到新笔记中。
- 笔记类型存储在 `type:` frontmatter 字段中。
- 大多数笔记以扁平的 `.md` 文件形式存放在 vault 根目录。类型定义存放在 `type/`。保存的视图存放在 `views/`。
- 任何包含 [[wikilinks]] 的 frontmatter 字段都会被视为关系。常见的名称包括 `Belongs to:`、`Related to:`、`Workspace:` 以及自定义关系名称。
- 以 `_` 开头的 frontmatter 属性通常是 Tolaria 管理的状态。除非用户明确要求更改，否则不要动它们。

## Notes

```yaml
---
type: Project
status: Active
icon: target
Workspace: "[[tolaria]]"
Belongs to:
  - "[[25q2]]"
Related to:
  - "[[person-luca-rossi]]"
aliases:
  - Tolaria work
url: https://example.com
---

# Ship Tolaria

Body content in Markdown.
```

## Types

类型定义是存放在 `type/` 中的常规笔记。新的类型定义请使用 `type: Type`：

```yaml
---
type: Type
icon: shapes
color: blue
sidebar label: Projects
template: |
  ## Outcome

  ## Next actions
---

# Project
```

有用的类型元数据包括 `icon`、`color`、`order`、`sidebar label`、`template`、`sort`、`view` 和 `visible`。

## Wikilinks

- [[filename]] 或 [[Note Title]] — 通过文件名或标题链接
- [[filename|display text]] — 带自定义显示文本
- 在 frontmatter 值和 Markdown 正文中均可使用

## Views

保存的视图存放在 `views/*.yml` 中，以 YAML 格式编写：

```yaml
name: Active Projects
icon: kanban
color: blue
sort: modified:desc
filters:
  all:
    - field: type
      op: equals
      value: Project
    - field: status
      op: equals
      value: Active
```

## Filenames

使用 kebab-case：`my-note-title.md`。每个文件一个笔记。

## What agents should do

- 使用上述 frontmatter 和 H1 约定来创建和编辑笔记。
- 在 `type/` 中创建和编辑类型文档。
- 添加或修改关系时不要破坏现有的 wikilinks。
- 在 `views/` 中创建和编辑保存的视图。
- 仅当用户要求更改 vault 层面的指导时才更新 `AGENTS.md`。

## What agents should avoid

- 不要从 `type/` 目录（专门用于类型定义）以外的文件夹推断笔记类型。
- 不要静默覆盖已有的自定义 `AGENTS.md`。
- 除非用户明确要求，否则不要覆盖用户编写的配置或特定于安装的应用文件。
