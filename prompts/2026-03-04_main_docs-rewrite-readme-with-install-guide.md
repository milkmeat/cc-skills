# Prompt Record

- Date: 2026-03-04 20:00
- Branch: main
- Commit: docs: rewrite README with installation and update guide

---

### 1

我已经push到github，我应该如何在其他项目中安装这个skill？是每个项目都要安装还是每台机器只需要安装一次？

> **Insight**
> - Claude Code 的 skill 安装有**两个层级**：项目级（`.claude/settings.json`）和用户级（`~/.claude/settings.json`）
> - 你当前项目用的是 `source: "directory"` 本地路径引用，推送到 GitHub 后，其他项目应改用 GitHub 源
> - `enabledPlugins` 控制哪些 skill 激活，marketplace 只是注册源

### 2

新的skill.md文件push到github之后，已经安装的机器如何更新

> **Insight**
> - `marketplace update` 更新的是 marketplace 源的索引，会拉取你仓库里最新的 SKILL.md 和 marketplace.json
> - 如果你新增了一个 skill（比如新建了 `my-new-skill/SKILL.md`），更新后还需要 `claude plugin install my-new-skill@cc-skills` 来启用它
> - 已安装的 skill 内容更新后，`marketplace update` 即可生效，不需要重新 install

### 3

把这些说明更新到readme.md

> **Insight**
> - README.md 和 CLAUDE.md 各有侧重：README 面向**使用者**（如何安装和使用），CLAUDE.md 面向 **Claude Code 自身**（项目约定和行为指令）
> - 两者内容有少量重叠（如项目结构），但不需要完全同步——CLAUDE.md 里的 commit 规范等对人类读者价值不大，放在 README 里反而增加噪音

**Files:** `README.md`

### 4

1. readme.md全部使用英文
2. 我的github用户名是milkmeat，替换一下

**Files:** `README.md`
