# Prompt Record

- Date: 2026-03-10
- Branch: main
- Commit: feat: add plan capture to prompt record in commit-with-prompts

---

### 1

更新commit-with-prompts,在assistant回复的会话中，如果有plan的话，也保存在prompt记录中

> **Insight**
> - **新增 Plan 字段**：在 prompt unit 的 5 个组成部分中，Plan 排在第 3 位（Context question 之后、Insight 之前），因为 plan 通常是对用户请求的第一层响应，先于具体实现产生的 insight
> - **触发条件**：Plan 块仅在 assistant 回复中包含 plan 时才出现——包括通过 plan mode（EnterPlanMode/ExitPlanMode）产生的计划，以及结构化的实现方案。这样能捕获架构决策和实现路径的"为什么"
> - **格式一致性**：Plan 和 Insight 一样使用 blockquote 格式（`>`），保持模板的视觉统一

**Files:** `commit-with-prompts/SKILL.md`
