# cc-skills

Custom Claude Code skills 仓库。每个子目录是一个独立的 skill。

## 项目结构

```
cc-skills/
├── .claude-plugin/marketplace.json   # 插件注册清单（新增 skill 需更新）
├── <skill-name>/SKILL.md             # Skill 定义文件
├── <skill-name>-workspace/           # 开发/测试工件（已 gitignore）
└── prompts/                          # Prompt 记录（自动生成，随代码一起提交）
```

## Skill 开发规范

- 每个 skill 一个目录，核心文件是 `SKILL.md`（YAML frontmatter + Markdown 指令）
- 使用 `document-skills:skill-creator` 来创建和迭代 skill
- 开发过程中的测试/评估产物放在 `<skill-name>-workspace/`，不纳入版本控制
- 新增 skill 后需更新 `.claude-plugin/marketplace.json` 注册插件，然后运行 `claude plugin marketplace update cc-skills`

## Commit 规范

- 使用 conventional commits 格式：`feat:`, `fix:`, `chore:`, `test:`, `docs:` 等
- When committing code, always use the commit-with-prompts skill to save user prompts alongside the commit.

## 语言

用户使用简体中文交流。Skill 文件和技术标识符使用英文。
