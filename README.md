# Skill Garden

[中文文档](README.zh-CN.md)

![License](https://img.shields.io/github/license/Apzx7/skill-garden)
![Skills](https://img.shields.io/badge/skills-growing-green)

> Give Claude Code a set of "skill packs" so it performs like a domain expert on demand.

## What is this?

[Skill Garden](https://github.com/Apzx7/skill-garden) is a collection of skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code).

**What is a Skill?** In short, a Skill is an "operation manual" for AI. It tells the AI: given a situation, follow these steps, and output in this format. With a Skill, the AI produces consistent, reliable results instead of improvising every time.

Think of it like a recipe book — anyone can cook a reliable dish with a recipe, not just a chef improvising on the spot.

## Why this project?

It started during a data security competition. Every time I needed model code, I had to write a long prompt explaining how to handle class imbalance, feature engineering, threshold tuning... repetitive and tedious.

So I thought: why not package these reusable instructions into fixed skill files? One `/` command and you're good to go.

Turns out this idea works across many domains, and so this repo was born.

## Getting Started

### Install

```bash
git clone https://github.com/Apzx7/skill-garden.git ~/.claude/skills/skill-garden
```

### Use

In Claude Code, type `/` followed by the skill name:

```text
/datasec-model-optimizer
```

Claude will execute the task following the skill's defined workflow.

## Available Skills

| Skill | Description |
| :---- | :---------- |
| [DataSec Model Optimizer](datasec-model-optimizer.md) | Data security competition model optimization — extreme class imbalance handling & modular code generation |

## Planned Skills

| Direction | Description |
| :-------- | :---------- |
| Historical Figures | Interactive conversations with historical personas |
| IELTS Chat | IELTS speaking & writing practice assistant |
| CTF | CTF competition problem-solving assistant |
| Code Audit | Security code audit analysis |

> Have an idea? Feel free to open an issue or submit a PR!

## License

[MIT](LICENSE)
