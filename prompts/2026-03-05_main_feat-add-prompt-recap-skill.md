# Prompt Record

- Date: 2026-03-05 12:00
- Branch: main
- Commit: feat: add prompt-recap skill for session-start context loading

---

### 1

Implement the following plan:

# prompt-recap Skill 实现计划

## Context

`commit-with-prompts` skill 在每次 commit 时将用户 prompt 保存到 `prompts/` 目录。但这些 prompt 历史在新 session 中没有被利用。`prompt-recap` 作为配套 skill，在 session 启动时读取近期 prompt 记录，提炼上下文要点，帮助 Claude 快速理解项目的近期动态和决策脉络。

两个 skill 形成闭环：`commit-with-prompts`（写入） → `prompt-recap`（读取）。

## 要创建/修改的文件

### 1. `prompt-recap/SKILL.md`（新建）

**YAML frontmatter:**
```yaml
name: prompt-recap
description: |
  Reviews recent prompt history at the start of every new Claude Code session.
  MUST be invoked at the beginning of each session, before responding to the user's first message.
  Reads prompt record files from the prompts/ directory (created by commit-with-prompts),
  synthesizes key context (user focus areas, technical choices, design decisions),
  and asks the user whether to continue with this context or start fresh.
```

**Markdown 指令主体（分步骤）：**

#### Step 1: Read Recent Prompt Records
- 列出 `prompts/` 目录下的所有 `.md` 文件
- 按文件名降序排列（自然按日期排序）
- 取最近 3 个文件，读取完整内容
- 如果 `prompts/` 目录不存在或为空，输出提示信息并结束（"没有找到 prompt 历史记录。"）

#### Step 2: Parse and Synthesize
- 从每个文件中提取：
  - 元数据：日期、分支、commit message
  - 用户 prompt 原文
  - Insight 内容（设计决策和推理）
  - 涉及的文件列表
- 综合提炼为三个维度：
  1. **近期关注点** — 用户最近在开发什么功能/修复什么问题
  2. **技术选择** — 做出了哪些技术/架构决策
  3. **设计由来** — 关键设计方案的背景和理由

#### Step 3: Display Recap
- 打印结构化摘要

- 每个维度 2-5 条要点，保持简洁可扫描
- 如果某个维度没有相关内容（比如纯 bugfix 没有设计决策），可以省略该维度

#### Step 4: Ask User
- 使用 AskUserQuestion 工具向用户确认：
  - 选项 1: **"继续"** — 从这些上下文继续工作（推荐）
  - 选项 2: **"加载更多"** — 读取更多历史记录（回到 Step 1，扩大范围）
  - 选项 3: **"全新开始"** — 不使用历史上下文

#### Setup Recommendation
- 在消费项目的 `CLAUDE.md` 中添加相应配置

### 2. `.claude-plugin/marketplace.json`（修改）

在 `plugins` 数组中添加 prompt-recap 的注册条目

> **Insight**
> - `commit-with-prompts` 的 prompt 记录文件有固定的元数据格式（`- Date:`, `- Branch:`, `- Commit:`），每个 prompt unit 用 `### N` 分隔，这是 `prompt-recap` 解析的基础
> - 文件名自带日期和分支信息（`YYYY-MM-DD_<branch>_<summary>.md`），所以降序排列文件名天然实现按时间倒序
> - 两个 skill 形成写-读闭环：`commit-with-prompts` 定义数据格式，`prompt-recap` 消费数据格式

**Files:** `prompt-recap/SKILL.md`, `.claude-plugin/marketplace.json`

### 2

新完成的那个skill.md里面不要出现中文。当然，如果用户的工作语言是其他语言，所提炼的内容摘要里面可以使用用户的工作语言

> **Insight**
> - **Skill 指令 vs 运行时输出的语言分离**：SKILL.md 作为"代码"应该用英文编写（与 `commit-with-prompts` 保持一致），而运行时生成的摘要内容则跟随用户的工作语言。这种分离让 skill 本身具有语言无关性——任何语言的用户安装后都能正常使用
> - 第 82 行新增的 `Use the user's working language for the recap content` 规则，将语言选择的决策权交给了运行时上下文，而不是硬编码在模板中

**Files:** `prompt-recap/SKILL.md`
