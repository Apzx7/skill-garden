# Skill Garden

[中文文档](README.zh-CN.md)

A curated collection of custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills — modular prompt workflows that extend Claude's capabilities for specific tasks.

## What is a Skill?

A skill is a Markdown file with YAML frontmatter that tells Claude Code how to handle a specific task. Each skill defines triggers, instructions, and standard operating procedures that Claude follows when invoked via `/skill-name`.

## Skills

| Skill                                    | Category      | Description                                                                                           |
| :--------------------------------------- | :------------ | :---------------------------------------------------------------------------------------------------- |
| [DataSec Model Optimizer](datasec-model-optimizer.md) | Security / ML | Data security competition model training & F1-score optimization with extreme class imbalance handling |

## Installation

Clone this repo into your Claude Code skills directory:

```bash
git clone https://github.com/Apzx7/skill-garden.git ~/.claude/skills/skill-garden
```

Or copy individual `.md` skill files to your skills directory.

## Usage

In Claude Code, type `/` followed by the skill's `name` value from its frontmatter:

```text
/datasec-model-optimizer
```

Claude will then follow the skill's SOP to generate optimized output.

## Creating Your Own Skill

A skill file follows this structure:

```yaml
---
name: my-skill-name
description: >
  One-line description of what this skill does.
  Triggers: "keyword1", "keyword2"
---
# Skill Title
## Step 1: ...
## Step 2: ...
```

- **name**: Used as the `/` command identifier
- **description**: Shown when browsing available skills; include trigger keywords
- **Body**: The detailed instructions and SOP Claude will follow

## Contributing

Contributions are welcome! Feel free to:

- Open an issue to suggest new skill ideas
- Submit a PR with your own skill files
- Improve existing skills

## License

[MIT](LICENSE)
