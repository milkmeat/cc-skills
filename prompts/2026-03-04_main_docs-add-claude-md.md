# Prompt Record

- Date: 2026-03-04 17:19
- Branch: main
- Commit: docs: add CLAUDE.md with project conventions

---

### 1

为本项目生成claude.md，方便我以后开发其他skills

> **Insight**
> - **精简优先**：CLAUDE.md 会被加载到每次对话的上下文中，过长会浪费 token。只放"必须遵守的规则"，不放"可以查文档的细节"
> - **commit-with-prompts 双保险**：在 CLAUDE.md 中写入 `always use the commit-with-prompts skill` 这一行，确保 skill 在每次 commit 时 100% 触发——即使 skill description 匹配失败，CLAUDE.md 的指令也会兜底

**Files:** `CLAUDE.md`
