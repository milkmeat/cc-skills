# Prompt Record

- Date: 2026-03-05 17:15
- Branch: main
- Commit: refactor: rename prompt-recap to recap and make it user-invocable

---

### 1

我已经安装了prompt-recap，好像没有正常工作？

> **Insight**
> prompt-recap 的设计是一个闭环系统的"读"端：commit-with-prompts 在提交时写入 prompt 记录，prompt-recap 在新会话开始时读取这些记录来恢复上下文。但"自动触发"依赖 CLAUDE.md 中的指令——Claude Code 在每次会话开始时会加载 CLAUDE.md，如果里面没有要求执行 prompt-recap，它就不会自动运行。

**Files:** `CLAUDE.md`

### 2

其他项目的用户在使用这两个skill的时候，也要加入这句话吗。不能自动hook吗

> **Insight**
> 1. 响应式 vs 主动式触发：Claude Code skill 的 description 本质上是一个"条件匹配器"——当用户输入匹配到描述中的关键词（如"commit"），skill 就能可靠触发。但"会话开始时自动执行"这种主动式触发没有关键词可匹配，必须靠 CLAUDE.md 的强制指令。把 prompt-recap 改成 /recap 命令后，触发变成了响应式的，用户输入 /recap 就是一个明确的关键词事件。
> 2. 用户体验的权衡：自动触发看似方便，但实际上每次开新会话都要等 recap 执行完才能开始工作，有时用户可能只是想快速做一件小事。改为按需调用 /recap，让用户自己决定何时需要上下文恢复，体验反而更好。

### 3

> **Q:** 要怎么改进安装体验？

既然不能自动化。提供一个/recap命令，让用户自己启动吧

**Files:** `recap/SKILL.md`, `.claude-plugin/marketplace.json`, `CLAUDE.md`, `README.md`, `commit-with-prompts/SKILL.md`
