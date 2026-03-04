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
