# Skill Garden

[English](README.md)

![License](https://img.shields.io/github/license/Apzx7/skill-garden)
![Skills](https://img.shields.io/badge/skills-growing-green)

> 为 AI 编程工具装上"技能包"，让它在特定领域表现得像专家一样。

## 这是什么？

[Skill Garden](https://github.com/Apzx7/skill-garden) 是一个面向 AI 编程工具的技能集合仓库，主要适配 [Claude Code](https://docs.anthropic.com/en/docs/claude-code)，也可作为 Cursor Rules、Cline 等工具的 prompt 参考。

**什么是 Skill？** 简单来说，Skill 就是一份写给 AI 的"操作手册"。你告诉它：遇到什么情况、按什么步骤做、输出什么格式。AI 拿到这份手册后，就能稳定地输出你想要的结果，而不是每次都即兴发挥。

更通俗点说：给 AI 一个 Skill，就像给一个人一本菜谱 —— 有了菜谱，谁都能做出稳定的菜品，而不需要每次都是大厨现场发挥。

## 为什么做这个？

起因是打数据安全竞赛时，每次都要手动写一大段 prompt 告诉 AI 怎么处理类别不平衡、怎么做特征工程、怎么调阈值……重复又繁琐。

于是想到：能不能把这些"反复要用的操作"封装成固定的技能文件？用的时候一个 `/` 命令就行。

后来发现这个思路可以延伸到更多领域，于是就有了这个仓库。

## 使用方法

### 安装

```bash
git clone https://github.com/Apzx7/skill-garden.git ~/.claude/skills/skill-garden
```

### 调用

在 Claude Code 中输入 `/` + skill 名称即可：

```text
/datasec-model-optimizer
```

Claude 会按照 Skill 中定义的流程自动完成任务。

## 现有技能

| 技能 | 说明 |
| :--- | :--- |
| [DataSec Model Optimizer](datasec-model-optimizer.md) | 数据安全竞赛模型优化 — 自动处理极端类别不平衡，生成模块化代码 |
| [AI English Trainer](ai-english-trainer.md) | 基于认知神经科学的 AI 英语陪练 — 预测编码 + 高密度反馈循环 |

## 计划中的技能

| 方向 | 说明 |
| :--- | :--- |
| 历史人物 | 与历史人物对话的交互式 skill |
| CTF | CTF 竞赛解题辅助 |
| 代码审计 | 安全代码审计分析 |

> 有想法？欢迎提 Issue 或 PR！

## 许可证

[MIT](LICENSE)
