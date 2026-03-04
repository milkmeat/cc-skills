# Prompt Record

- Date: 2026-03-04 17:09
- Branch: main
- Commit: feat: add context question (Q:) support to prompt records

---

### 1

> **Q:** 两个选项：1) 跳过 commit 2) 提交 workspace 目录 3) 将 workspace 加入 .gitignore

3

> **Insight**
> 使用 `*-workspace/` 而非 `commit-with-prompts-workspace/`，这样未来开发其他 skill 时也会自动被忽略，无需反复修改 .gitignore。

**Files:** `.gitignore`

### 2

你上面说"Skill 已触发！让我按照 commit-with-prompts 的工作流执行。"，也生成了prompt记录表格。但是我怎么没在git log里面看到prompt记录

> **Insight**
> 这次失误暴露了 skill 的一个盲区：当 skill 在 session 中途才安装时，之前的 commit 没有 prompt 记录。skill 的当前设计假设它从 session 开始就已经可用。可能需要考虑一个"补录"机制。

**Files:** `prompts/2026-03-04_main_feat-add-commit-with-prompts-skill.md`

### 3

做以下改进：
1. 如果user prompt是做选择或者回答问题。把问题原文也记录下来。例如 上面日志里面的 "###2 prompts/"

> **Insight**
> "自包含性"原则：prompt 记录文件可能在数周甚至数月后被阅读。每一条记录都应该独立可理解，不需要读者去翻找前后文。`Q:` 块就像数据库中的外键——把引用的上下文直接内联进来，确保数据不会"悬空引用"。

**Files:** `commit-with-prompts/SKILL.md`
