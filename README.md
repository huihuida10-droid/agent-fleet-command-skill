# Agent Fleet Command

![Claude Skill](https://img.shields.io/badge/Claude-Skill-purple)
![Cursor Compatible](https://img.shields.io/badge/Cursor-Compatible-blue)
![Copilot Ready](https://img.shields.io/badge/Copilot-Ready-green)

多智能体编排技能：主智能体指挥调度，子智能体分阶段并行实现，每个子智能体自建审查者。增量 + 最终一致性检查确保质量。支持 Claude Code、Cursor、Copilot。

## 安装

### npx skills（推荐）

```bash
npx skills add https://github.com/your-username/agent-fleet-command
```

### Claude Code

```bash
/plugin marketplace add your-username/agent-fleet-command
```

### 克隆 / 复制

将 `skills/` 文件夹复制到对应 Agent 的 skill 目录：

| Agent | Skill 目录 |
|-------|-----------|
| Claude Code | `~/.claude/skills/` |
| Cursor | `~/.cursor/skills/` |
| OpenCode | `~/.config/opencode/skills/` |
| OpenAI Codex | `~/.codex/skills/` |

## 技能列表

| 技能 | 说明 |
|------|------|
| **agent-fleet-command** | 完整编排技能：文档创建、分阶段派发、自管理审查链、一致性检查 |

## 工作原理

### 阶段 0：初始化

主智能体创建：
- `project-overview.md` — 架构大纲，模块划分，依赖关系图
- `task-{name}.md` — 每个任务的详细设计，含需求和约束

### 阶段 N：执行

每个阶段内（独立任务并行执行）：

1. 子智能体阅读两份文档 + 检查 skill 库
2. 实现代码（TDD）
3. 自审
4. 创建审查子智能体
5. 审查者对照两份文档检查
6. 不通过 → 修复 → 重新审查 → 循环直到通过
7. 提交到主智能体

### 一致性检查

- **增量检查：** 每个阶段完成后，主智能体检查跨任务一致性
- **最终检查：** 所有阶段完成后，完整的一致性 + 健康性检查

## 目录结构

```
agent-fleet-command/
├── README.md
├── AGENTS.md
├── LICENSE
├── .gitignore
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .cursor-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── .github/
│   └── copilot-instructions.md
└── skills/
    ├── llms.txt
    └── agent-fleet-command/
        ├── SKILL.md
        ├── orchestrator-prompt.md
        ├── implementer-prompt.md
        ├── reviewer-prompt.md
        └── test-scenario.md
```

## 角色约束

| 角色 | 能做 | 不能做 |
|------|------|--------|
| 主智能体 | 创建文档、派发任务、一致性检查 | 读写代码文件 |
| 子智能体 | 实现分配的任务、只修改范围内的文件 | 修改范围外的文件 |
| 审查子智能体 | 审查代码、生成问题报告 | 修改任何代码 |

## 许可证

MIT
