# cc-skills

## Background

In AI-assisted development, **prompts are the real source code**. The code itself is a byproduct — it's the prompts that capture intent, constraints, trade-offs, and the reasoning that shaped every decision. Treating prompts as disposable is like writing code without version control: you lose the ability to understand *why* things are the way they are.

This repository provides two Claude Code skills that address this with a single closed-loop design:

- **commit-with-prompts** — Preserves prompts in version control. Every time you commit, the prompts that drove those code changes are saved alongside them in `prompts/`. Prompts become first-class artifacts in your git history, not ephemeral chat messages.

- **prompt-recap** — Loads prompt history at session start. When you begin a new session, recent prompt records are read, synthesized, and presented as a structured recap — so Claude already knows what you've been working on, what decisions were made, and why. No more cold starts.

One writes, the other reads. Together they make prompt history both **durable** (survives across sessions and machines) and **actionable** (automatically informs the next session).

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

# 2. Install skills
claude plugin install commit-with-prompts@cc-skills
claude plugin install prompt-recap@cc-skills
```

Global config is written to `~/.claude/settings.json` and applies to all projects on the machine.

### Per-Project Install (current project only)

Per-project config is written to `<project>/.claude/settings.json` and can be committed to git for team sharing.

### Comparison

| | Global | Per-Project |
|---|---|---|
| **Config location** | `~/.claude/settings.json` | `<project>/.claude/settings.json` |
| **Scope** | All projects on the machine | Current project only |
| **Best for** | Personal utility tools | Team collaboration, project-specific skills |
| **Commit to git** | No | Yes, for team sharing |

## Usage

### commit-with-prompts

#### How It Triggers

The `commit-with-prompts` skill activates **automatically** whenever you ask Claude Code to commit. Any of these will trigger it:

- `commit`, `git commit`, `/commit`
- `提交`, `提交代码`
- Any natural language request that implies committing

No special syntax or flags needed — just commit as you normally would.

#### What Happens

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

#### Output File Format

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

### prompt-recap

#### How It Triggers

`prompt-recap` is designed to run **at the start of every new Claude Code session**, before responding to the user's first message. To enable automatic activation, add the following to your project's `CLAUDE.md`:

```
At the start of each session, use the prompt-recap skill to review recent prompt history before responding to the user's first message.
```

#### What Happens

When a new session starts, the skill:

1. **Reads** the 3 most recent prompt record files from `prompts/` (sorted by filename, newest first)
2. **Extracts** metadata (date, branch, commit message), user prompts, insights, and affected files from each record
3. **Synthesizes** the information into three dimensions:
   - **Recent Focus** — what the user has been working on (features, bugs, areas)
   - **Technical Choices** — architectural and technical decisions made
   - **Design Rationale** — background and reasoning behind key design choices
4. **Displays** a structured recap:

```
Prompt Recap — Last 3 commits
═══════════════════════════════════════

Recent Focus
  - prompt history: added prompt-recap skill to load context at session start
  - ...

Technical Choices
  - language separation: SKILL.md in English, runtime output follows user's language
  - ...

Files touched: prompt-recap/SKILL.md, .claude-plugin/marketplace.json
Time range: 2026-03-04 ~ 2026-03-05
─────────────────────────────────────
```

5. **Asks** the user how to proceed:
   - **Continue** (recommended) — accept the context and start working
   - **Load more** — expand to 8 files for deeper history
   - **Fresh start** — discard context and begin with a clean slate

If the `prompts/` directory doesn't exist or is empty, the skill outputs "No prompt history found." and stops.

### Closed-Loop Workflow

The two skills form a **write → read** loop:

```
commit-with-prompts                    prompt-recap
(writes on commit)                     (reads on session start)
       │                                      │
       │   ┌──────────────────────┐           │
       └──►│  prompts/*.md files  │───────────┘
            └──────────────────────┘
```

| | commit-with-prompts | prompt-recap |
|---|---|---|
| **When** | Every git commit | Every new session |
| **Action** | Saves user prompts + insights to `prompts/` | Reads recent records, synthesizes context |
| **Purpose** | Preserve the "why" behind code changes | Eliminate cold starts in new sessions |

**Typical scenario:**

1. You work on a feature across multiple commits — each commit saves the prompts that drove it
2. You close the session and come back later (or context resets)
3. `prompt-recap` reads the recent prompt history and presents a recap
4. You choose "Continue" and Claude already knows what you've been working on, what decisions were made, and why

**Setup for both skills** — add these lines to your project's `CLAUDE.md`:

```
When committing code, always use the commit-with-prompts skill to save user prompts alongside the commit.
At the start of each session, use the prompt-recap skill to review recent prompt history before responding to the user's first message.
```

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
