# cc-skills

Custom Claude Code skills repository. Each subdirectory is an independent skill.

## Project Structure

```
cc-skills/
├── .claude-plugin/marketplace.json   # Plugin registry manifest
├── <skill-name>/SKILL.md             # Skill definition file
├── <skill-name>-workspace/           # Dev/test artifacts (gitignored)
└── prompts/                          # Prompt records (auto-generated, committed with code)
```

## Installation

### Global Install (Recommended — once per machine, available to all projects)

```bash
# 1. Add marketplace
claude plugin marketplace add milkmeat/cc-skills

# 2. Install skill
claude plugin install commit-with-prompts@cc-skills
```

Global config is written to `~/.claude/settings.json` and applies to all projects on the machine.

### Per-Project Install (current project only)

```bash
cd /path/to/your-project

claude plugin marketplace add milkmeat/cc-skills
claude plugin install commit-with-prompts@cc-skills
```

Per-project config is written to `<project>/.claude/settings.json` and can be committed to git for team sharing.

### Comparison

| | Global | Per-Project |
|---|---|---|
| **Config location** | `~/.claude/settings.json` | `<project>/.claude/settings.json` |
| **Scope** | All projects on the machine | Current project only |
| **Best for** | Personal utility tools | Team collaboration, project-specific skills |
| **Commit to git** | No | Yes, for team sharing |

## Usage

### How It Triggers

The `commit-with-prompts` skill activates **automatically** whenever you ask Claude Code to commit. Any of these will trigger it:

- `commit`, `git commit`, `/commit`
- `提交`, `提交代码`
- Any natural language request that implies committing

No special syntax or flags needed — just commit as you normally would.

### What Happens

When you commit, the skill augments the standard git commit workflow:

1. **Collects** all substantive user prompts from the current session (since the last commit)
2. **Drafts** the commit message as usual (`git status`, `git diff`, etc.)
3. **Creates** a prompt record file in `prompts/` capturing each prompt along with optional context (insights, affected files, Q&A context)
4. **Commits** both the code changes and the prompt record in a single atomic commit
5. **Reports** a summary to the console:

```
Prompt record: prompts/2026-03-04_main_feat-add-user-auth.md
  5 prompts | Topics: user authentication, JWT tokens, logout endpoint
```

Trivial messages like "yes", "ok", "commit", "确认" are automatically skipped — only substantive prompts that carry design intent are recorded.

### Output File Format

Prompt records are saved as Markdown files under `prompts/` at the project root.

**Filename convention:** `YYYY-MM-DD_<branch>_<summary>.md`

| Segment | Description | Example |
|---|---|---|
| `YYYY-MM-DD` | Date of the commit | `2026-03-04` |
| `<branch>` | Git branch (`/` replaced with `-`) | `main`, `feature-auth` |
| `<summary>` | Slug from commit message (≤50 chars) | `feat-add-user-auth` |

If a filename already exists, `-2`, `-3`, etc. is appended.

**File structure:**

```markdown
# Prompt Record

- Date: 2026-03-04 16:58
- Branch: main
- Commit: feat: add user authentication

---

### 1

<user prompt text — preserved faithfully, never summarized>

> **Insight**
> <key insight from Claude's response, if any>

**Files:** `src/auth.py`, `src/middleware.py`

### 2

> **Q:** <the question Claude asked that this prompt is answering>

<user's answer>

**Files:** `src/config.py`

### 3

<standalone instruction — no Q: block needed>
```

**Elements per prompt unit:**

| Element | Required | Description |
|---|---|---|
| Numbered heading (`### N`) | Yes | Sequential prompt number |
| User prompt text | Yes | Original user message, preserved verbatim |
| `> **Q:**` block | Only when answering a question | Provides context when the user's message is a reply to Claude's question |
| `> **Insight**` block | Only when present | Captures the `★ Insight` section from Claude's response |
| `**Files:**` line | Only when files changed | Lists files created, modified, or deleted in response to that prompt |

## Updating

After pushing new or modified skills to GitHub, run on any machine that has the marketplace installed:

```bash
claude plugin marketplace update cc-skills
```

- Existing installed skills are updated automatically — no need to re-install.
- For newly added skills, run `claude plugin install <skill-name>@cc-skills` after updating.

## Skill Development

Use `document-skills:skill-creator` to create and iterate on skills.

After adding a new skill:

1. Update `.claude-plugin/marketplace.json` to register the plugin
2. Run `claude plugin marketplace update cc-skills`

Dev tools reference: [skill-creator](https://github.com/anthropics/skills/blob/main/README.md)
