# Skill Garden

[English](README.md)

一个精选的 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 技能集合 — 模块化的 prompt 工作流，扩展 Claude 处理特定任务的能力。

## 什么是 Skill？

Skill 是一个带 YAML frontmatter 的 Markdown 文件，它告诉 Claude Code 如何处理特定任务。每个 skill 定义了触发关键词、指令和标准操作流程（SOP），Claude 在通过 `/skill-name` 调用时会遵循这些流程。

## 技能列表

| 技能 | 分类 | 说明 |
| :--- | :--- | :--- |
| [DataSec Model Optimizer](datasec-model-optimizer.md) | 安全 / 机器学习 | 数据安全竞赛模型训练与 F1-score 极致优化，自动处理极端类别不平衡 |

## 安装

将本仓库克隆到你的 Claude Code 技能目录：

```bash
git clone https://github.com/Apzx7/skill-garden.git ~/.claude/skills/skill-garden
```

或者只复制你需要的 `.md` 技能文件到技能目录。

## 使用

在 Claude Code 中，输入 `/` 加上 skill 的 `name` 值：

```text
/datasec-model-optimizer
```

Claude 将按照该 skill 的标准操作流程生成优化后的输出。

## 创建你自己的 Skill

Skill 文件结构如下：

```yaml
---
name: my-skill-name
description: >
  一句话描述该 skill 的功能。
  Triggers: "关键词1", "关键词2"
---
# 技能标题
## 步骤一：...
## 步骤二：...
```

- **name**：用作 `/` 命令的标识符
- **description**：浏览可用技能时显示的描述，包含触发关键词
- **Body**：Claude 将遵循的详细指令和标准操作流程

## 贡献

欢迎贡献！你可以：

- 提交 Issue 建议新的 skill 创意
- 提交 PR 添加你自己的 skill 文件
- 改进现有的 skill

## 许可证

[MIT](LICENSE)
