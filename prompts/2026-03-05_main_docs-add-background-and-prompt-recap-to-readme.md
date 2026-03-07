# Prompt Record

- Date: 2026-03-05 12:30
- Branch: main
- Commit: docs: add background section and prompt-recap documentation to README

---

### 1

更新readme文件，把使用方法，使用效果，产出文件的格式说明一下

更新readme文件，记录prmpt-recap的 1. 安装方法 2. 使用方法 3. 与commit-with-promts的配合效果及使用场景

> **Insight**
> - 当前 README 完全围绕 `commit-with-prompts` 展开，需要将结构从"单 skill 文档"调整为"多 skill 文档"
> - README 遵循英文写作（从 prompt 记录中可以看到用户之前明确要求的）
> - 需要在 Installation 段添加 prompt-recap 的安装命令，Usage 段重构为 skill 分区，并增加闭环工作流说明

**Files:** `README.md`

### 2

在readme最前面增加一章background，强调两个事情：
1. prompt是新范式下的*真正*源代码，不应该用过就丢，应该保存到source control。这是commit-with-prompts的作用
2. 历史的prompts在继续开发时非常有用，所以我们让他自动载入。这是prompt-recap的作用
3. 我希望一举两得，所以开发了这两个skill

> **Insight**
> - 删掉了原来的 "Custom Claude Code skills repository..." 一句话简介——Background 章节的第一段已经承担了这个角色，且传递了更强的立场。保留它会显得重复
> - "Treating prompts as disposable is like writing code without version control" 这个类比把 prompt 版本控制的必要性锚定在了开发者已经理解的概念上，降低了认知门槛

**Files:** `README.md`
