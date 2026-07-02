---
_width: wide
---
# GBrain 安装与使用教程

## 一、GBrain 是什么

GBrain 是一个 **Postgres 原生的个人知识大脑**，由 Y Combinator 总裁 Garry Tan 开源。它不是普通的搜索工具，而是给 AI agent 加上一层「大脑」——除了关键字 + 向量检索，还能做**综合答案（synthesis）**、**知识图谱遍历**、**缺口分析（gap analysis）**。

一句话区分：

| 工具              | 返回                           |
| --------------- | ---------------------------- |
| `gbrain search` | 排序后的原始页（chunk 级混合检索）         |
| `gbrain think`  | 跨页综合的答案 + 引用 + 「大脑还不知道什么」的提示 |

技术栈：Bun + TypeScript，默认 PGLite（Postgres 17 WASM，零配置），可平滑迁移到 Postgres + pgvector。当前版本 `v0.42.53.0`。

***

## 二、三种安装路径，按需选一种

| 场景                 | 路径                                          | 何时选                   |
| ------------------ | ------------------------------------------- | --------------------- |
| **A. CLI 独立**      | `bun install -g` + PGLite                   | 个人单机、想自己掌控每一步         |
| **B. 接入 AI Agent** | Claude Code / Codex / Cursor                | 让编码 agent 拥有记忆        |
| **C. 全自主部署**       | OpenClaw / Hermes + 43 skills + dream cycle | 24/7 自动摄入、整理、富化的"梦循环" |

### 前置：安装 Bun（必需）

```bash
curl -fsSL https://bun.sh/install | bash
export PATH="$HOME/.bun/bin:$PATH"
# 建议把上面这行加到 ~/.zshrc 或 ~/.bashrc
bun --version   # 验证
```

***

### 路径 A：CLI 独立安装（推荐入门）

```bash
# 1) 全局安装 gbrain
bun install -g github:garrytan/gbrain  
gbrain --version   # 应打印版本号

# 2) 初始化大脑（PGLite，2 秒完成，无需 Docker）
gbrain init --pglite

# 3) 健康检查
gbrain doctor
```

**如果** `bun install -g` **后** `gbrain` **不可用或** `doctor` **报** `schema_version: 0`（Bun 偶尔会跳过 postinstall hook），按官方 issue #218 的恢复方案：

```bash
gbrain apply-migrations --yes
```

仍失败则走确定性路径：

```bash
git clone https://github.com/garrytan/gbrain.git ~/gbrain && cd ~/gbrain
bun install && bun link
```

***

### 路径 B：给 Claude Code / Codex 接入记忆层

**最轻量方案（零服务器、零 token、零隧道）：**

```bash
# 先按路径 A 完成 init
gbrain init --pglite

# 接入 Claude Code
claude mcp add gbrain -- gbrain serve

# 或接入 Codex
codex mcp add gbrain -- gbrain serve
```

这样 agent 会把 `gbrain serve` 作为 stdio 子进程启动，直接读写本地大脑。

**如果大脑在远程主机上**（已通过 `gbrain serve --http` 暴露），用一条命令连：

```bash
# Claude Code
gbrain connect https://your-host/mcp --token gbrain_xxx --install

# Codex（token 走 $GBRAIN_REMOTE_TOKEN 环境变量，不落配置文件）
gbrain connect https://your-host/mcp --token gbrain_xxx --agent codex --install
```

`--install` 会自动写入配置并做一次 token smoke-test。

***

### 路径 C：全自主部署（agent 平台）

把以下链接粘到 OpenClaw / Hermes / Cursor / Claude Code 等任意能读 URL 的 agent 里：

```text
Retrieve and follow the instructions at:
https://raw.githubusercontent.com/garrytan/gbrain/master/INSTALL_FOR_AGENTS.md
```

agent 会自己完成 9 步安装、询问 API key、加载 43 个 skill、配置 dream cycle。全程约 30 分钟。

***

## 三、配置 API Key

GBrain 默认用 ZeroEntropy 做嵌入 + 重排（v0.36.2.0 起），OpenAI/Voyage/Anthropic 为可选回退。

```bash
# 写入 shell profile 或 .env
export ZEROENTROPY_API_KEY=ze-...      # 默认嵌入 + 重排（必需）
export OPENAI_API_KEY=sk-...           # 向量检索回退 + chat 模型
export ANTHROPIC_API_KEY=sk-ant-...    # 可选，提升 query expansion 质量
```

或写入配置文件 `~/.gbrain/config.json`：

```bash
gbrain config set zeroentropy_api_key ze-...
gbrain config set anthropic_api_key sk-ant-...
```

> 没有任何嵌入 provider 时，关键字检索仍可用；没有 Anthropic 时检索可用但跳过 query expansion。

***

## 四、关键一步：选择检索模式（不要跳过）

`gbrain init` 会自动套一个默认模式，但成本差距最大 **25 倍**，必须自己确认。先看成本矩阵（按 10K queries/月）：

```text
                  Haiku 4.5     Sonnet 4.6    Opus 4.7
                  ($1/M)        ($3/M)        ($5/M)
  conservative    $40/mo        $120/mo       $200/mo
  balanced        $100/mo       $300/mo       $500/mo
  tokenmax        $200/mo       $600/mo       $1,000/mo
```

三种模式的区别：

| 模式             | 预算  | LLM 扩展 | chunk 数 | 适用                 |
| -------------- | --- | ------ | ------- | ------------------ |
| `conservative` | 4K  | 关      | 10      | Haiku 子 agent、成本敏感 |
| `balanced`     | 12K | 关      | 25      | Sonnet 甜点          |
| `tokenmax`（默认） | 无   | 开      | 50      | Opus / 前沿模型        |

切换：

```bash
gbrain config set search.mode balanced
gbrain search modes    # 验证当前模式
```

***

## 五、把数据灌进大脑

### 1. 批量导入现有 Markdown

```bash
gbrain import ~/my-notes/ --no-embed   # 导入文件
gbrain embed --stale                   # 生成向量
```

### 2. 单条快速捕获

```bash
gbrain capture "刚才想到的产品灵感"
gbrain capture --file ./today.md
echo "从管道传入" | gbrain capture --stdin

# --quiet 只返回 slug，便于脚本化
SLUG=$(gbrain capture "..." --quiet)
```

默认 slug 形如 `inbox/YYYY-MM-DD-<hash8>`，集中落在 inbox 便于整理。

### 3. 实时同步 git 仓库

```bash
gbrain sync --repo ~/brain            # 单次同步
gbrain sync --watch                   # 持续监听
```

### 4. 接知识图谱（已有 brain 时）

```bash
gbrain extract links --source db --dry-run | head -20   # 预览
gbrain extract links --source db                        # 落库
gbrain extract timeline --source db                     # 时间线
gbrain stats                                            # 验证 links > 0
```

> **Obsidian 用户注意：** 跨文件夹的裸 [[note-name]] 默认不连边。开启 basename 解析：

```bash
gbrain config set link_resolution.global_basename true
gbrain extract links --source db    # 重跑让新边落库
```

***

## 六、查询大脑

### 原始检索：`search`

快速、无 LLM 成本，返回排序后的页：

```bash
gbrain search "portfolio 里谁在做 AI agent?"
gbrain search "<query>" --explain    # 显示每阶段的得分归因
```

### 综合答案：`think`

跨页综合 + 引用 + 缺口分析：

```bash
gbrain think "明天和 Alice 开会，我需要知道什么?"
```

返回示例形态：

```text
Alice 是 Acme（B 轮 fintech）的工程负责人。最近一次沟通是
4 月 22 日的定价沟通，三件事悬而未决：
1. 她欠你新 tier 的安全审计（截止 5 月 1 日，无后续）
2. 你承诺 500 seat 的定价（4 月 25 日发出，未回复）
3. 她提到在招 CISO，你说介绍人脉

提醒：自 4 月 22 日起 6 周没有关于 Alice 或 Acme 的新内容。
可能通过邮件/Slack 回复了，大脑看不到。建议当面确认。
```

### 图谱遍历

```bash
gbrain graph-query people/alice --depth 2
```

***

## 七、让大脑自己变聪明：Dream Cycle

这是 GBrain 区别于「检索盒子」的核心。每夜跑一次 8 阶段维护：

```bash
gbrain dream
```

包含：实体扫描、引用修复、记忆合并、对话综合、跨会话模式识别、矛盾检测。

### 推荐的 cron 节奏

```cron
# 每 15 分钟：增量同步 + 嵌入
*/15 * * * * gbrain sync --repo ~/brain && gbrain embed --stale

# 每夜：dream cycle
0 3 * * * gbrain dream

# 每天：检查更新（不自动升级，只通知）
0 9 * * * gbrain check-update --json

# 每周：体检
0 10 * * 0 gbrain doctor --json && gbrain embed --stale
```

或者用内置守护进程（无需 cron）：

```bash
gbrain autopilot --install
```

***

## 八、Skill 系统（43 个内置技能）

如果你在 agent 平台工作区（路径 C），把 skill 脚手架进仓库：

```bash
cd /path/to/agent/workspace
gbrain skillpack scaffold --all      # 拷贝 43 个 skill + RESOLVER.md
```

> 这是 v0.36.0.0 起的推荐做法。老的 `skillpack install` managed-block 模式已废弃，从老版本升级跑一次 `gbrain skillpack migrate-fence`。

最该立即采用的三个 skill：

1. **signal-detector** — 每条入站消息都触发，捕获想法和实体
2. **brain-ops** — 每次响应前先查大脑
3. **conventions/quality** — 引用格式、反链铁律、来源归属

skill 调度看 `skills/RESOLVER.md`。

***

## 九、日常运维命令速查

| 命令                              | 用途                     |
| ------------------------------- | ---------------------- |
| `gbrain doctor [--json]`        | 全量健康检查                 |
| `gbrain doctor --fix`           | 自动修可修的问题               |
| `gbrain models`                 | 查看当前各 AI 模型的配置         |
| `gbrain models doctor`          | 每个配置模型发 1 token 探测     |
| `gbrain stats`                  | 大脑统计（页数、链接数等）          |
| `gbrain orphans`                | 零反链的孤立页                |
| `gbrain onboard --check --json` | 5 维度大脑健康建议             |
| `gbrain upgrade`                | 自更新 + schema 迁移 + 升级提示 |
| `gbrain apply-migrations --yes` | 手动跑 schema 迁移          |
| `gbrain repair-jsonb --dry-run` | 修复 v0.12.0 双重编码 JSONB  |

***

## 十、常见坑与排障

**Q:** `gbrain import` **报** `expected N dimensions, not M`\
A: 跑 `gbrain doctor`，它会打印精确的 `gbrain config set ...` 或 `gbrain retrieval-upgrade` 修复命令。一般无需删 `~/.gbrain`。

**Q: 初始化时没设 API key，怎么补救？**\
A: 设 `OPENAI_API_KEY`（或 `ZEROENTROPY_API_KEY` / `VOYAGE_API_KEY`）后重跑；或显式指定：

```bash
gbrain config set embedding_model openai:text-embedding-3-small
```

**Q: cron 同步在大型 brain 上超时？**\
A: v0.41.13.0 起推荐的 per-source 循环 + 系统 `timeout`：

```bash
gbrain sync --break-lock --all --max-age 1800
for src in $(gbrain sources list --json | jq -r '.[].id'); do
  timeout 600 gbrain sync --source "$src" --timeout 540 || true
done
```

**Q: PGLite 大 brain 上** `gbrain sync` **卡死？**\
A: PGLite 单写者，跑大同步前先停 `gbrain serve`。定位卡死文件：

```bash
GBRAIN_SYNC_TRACE=1 gbrain sync --no-pull --no-embed --yes
```

最后一行 `[sync] begin import: <path>` 没跟着完成行，就是卡住的那个文件。

***

## 十一、迁移与扩容

**PGLite → Postgres / Supabase**（brain 超 ~1000 文件时考虑）：

```bash
gbrain migrate --to supabase
```

**瘦客户端模式**（连别人的 brain，不跑本地引擎）：

```bash
gbrain init --mcp-only
```

**HTTP MCP server（多用户 / 团队 brain）：**

```bash
gbrain serve --http      # 自带 OAuth 2.1 + /admin 管理面板
```

部署参考 `docs/mcp/DEPLOY.md`（ngrok / Railway / Fly.io）。

***

## 十二、下一步学习路径

| 想做什么            | 看哪里                                        |
| --------------- | ------------------------------------------ |
| 完整个人 brain 从零搭  | `docs/tutorials/personal-brain.md`         |
| 团队 / 公司 brain   | `docs/tutorials/company-brain.md`          |
| 编码 agent 接入全流程  | `docs/tutorials/connect-coding-agent.md`   |
| 自己写 schema pack | `docs/schema-author-tutorial.md`           |
| MCP 各客户端配置      | `docs/mcp/` 目录                             |
| 嵌入 provider 选型  | `docs/integrations/embedding-providers.md` |

官方文档地图：`llms.txt`（精简）或 `llms-full.txt`（含核心文档内联）。

***

## 最小可行路径

只需要三行：

```bash
bun install -g github:garrytan/gbrain
gbrain init --pglite
gbrain think "你脑子里想问的第一个问题"
```
