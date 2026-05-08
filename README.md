# Skill Garden

A collection of custom [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills — modular prompt workflows that extend Claude's capabilities for specific tasks.

## Skills

| Skill                                                 | Description                                                              |
| :---------------------------------------------------- | :----------------------------------------------------------------------- |
| [DataSec Model Optimizer](datasec-model-optimizer.md) | 数据安全竞赛模型训练与 F1-score 极致优化专家，自动处理极端类别不平衡，生成模块化 Python 代码 |

## Installation

```bash
# Clone the repo into your skills directory
git clone https://github.com/Apzx7/skill-garden.git ~/.claude/skills/skill-garden
```

Or copy individual `.md` files to your skills directory.

## Usage

In Claude Code, type `/` followed by the skill's `name` field from its frontmatter:

```text
/datasec-model-optimizer
```

Claude will then follow the skill's SOP to generate optimized code.

## Project Structure

```text
skill-garden/
├── README.md
├── LICENSE
└── datasec-model-optimizer.md   # 数据安全赛模型优化 skill
```

## Contributing

Feel free to open issues or submit PRs to add new skills.

## License

[MIT](LICENSE)
