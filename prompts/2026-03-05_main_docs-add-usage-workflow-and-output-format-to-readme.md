# Prompt Record

- Date: 2026-03-05 10:00
- Branch: main
- Commit: docs: add usage, workflow, and output format sections to README

---

### 1

更新README.md，把使用方法，使用效果，产出文件的格式说明一下

> **Insight**
> - 当前 README 已有安装和更新说明，但缺少**使用方法**（怎么触发）、**使用效果**（会发生什么）、**产出文件格式**（prompt record 的结构）这三部分
> - 从 prompt 记录历史看，用户之前明确要求 README 使用英文，所以新增内容也将保持英文
> - prompt record 的格式规范全部定义在 `SKILL.md` 中，需要从中提炼出面向使用者的说明
> - **Usage > How It Triggers** — 列出了所有触发方式，强调用户无需任何特殊操作，像平常一样说"commit"即可自动激活
> - **Usage > What Happens** — 用 5 步流程说明了 skill 在 commit 时做了什么，包括收集 prompt、创建记录文件、原子提交、控制台摘要输出
> - **Output File Format** — 完整的文件命名规范和内容结构说明，包括文件名的三段式约定（日期\_分支\_摘要）、Markdown 模板示例、以及每个 prompt unit 中各元素（Q:、Insight、Files）的出现条件

**Files:** `README.md`
