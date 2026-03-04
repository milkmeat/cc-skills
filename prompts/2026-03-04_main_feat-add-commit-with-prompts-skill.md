# Prompt Record

- Date: 2026-03-04 16:58
- Branch: main
- Commit: feat: add commit-with-prompts skill with prompt record

---

### 1

使用 skill creator帮我开发一个 skill: commit-with-prompts

背景：将prompt视为真正的源代码。不应将prompt用过就忘掉，而应该保留prompts和会话的历史记录。自动将prompt纳入source control中管理起来。

目标：
- 在claude code中commit的时候，除了完成原有动作之外，把本session中使用过的user prompt(仅user部分)，截止至上一次commit起，全部保存到文件与源代码一起提交。
- hook到 commit 动作上自动执��，不需要用户特意指定
- 在控制台上输出所保存的prompt记录文件名，并对内容进行摘要
- 目前仅需要支持git

待定：
- 存放prompt的目录及文件名格式我还没决定，请给出建议

> **Insight**
> - **触发可靠性**：Skill 的 `description` 字段是 Claude 决定是否调用的唯一依据。描述必须足够"aggressive"才能在用户说"commit"/"提交"时可靠触发，但又不能过于宽泛导致误触发。
> - **增强 vs 替换**：这个 skill 应该**增强**而非替换 Claude Code 内置的 commit 工作流。先保存 prompt，再走标准 commit 流程，确保不丢失任何原有功能。

**Files:** `commit-with-prompts/SKILL.md`

### 2

prompts/

### 3

时间戳（到天) + 分支名 + commit message 摘要

### 4

1. 每一小节除了user prompt之外，还可以保留claude回复中的"Insight"小节内容，并列出受本prompt影响的文件名

> **Insight**
> - 每个 prompt 不再是孤立的文字，而是一个结构化的变更单元：Intent（用户 prompt）、Reasoning（Insight）、Impact（Files）
> - 三者结合起来，形成了完整的变更审计链条。以后回溯代码历史时，不仅能看到"改了什么"（git diff），还能看到"为什么改"（prompt）和"怎么想的"（insight）

**Files:** `commit-with-prompts/SKILL.md`

### 5

把这个skill安装到当前工程中使用

> **Insight**
> - **Marketplace** 是插件的来源（可以是 GitHub repo 或本地目录），通过 `.claude-plugin/marketplace.json` 定义
> - 安装命令格式：`claude plugin install <plugin>@<marketplace> --scope <project|user|local>`
> - 本地开发时用目录路径注册 marketplace，修改 skill 后只需 `claude plugin marketplace update cc-skills` 即可同步

**Files:** `.claude-plugin/marketplace.json`
