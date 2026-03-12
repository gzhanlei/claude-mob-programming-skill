# Claude Mob Programming Skill

一个用于 [Claude Code](https://claude.ai/code) 的 Mob Programming Skill，组建三人开发团队进行 TDD（测试驱动开发）。

## 简介

这个 skill 创建一个虚拟的 Mob Programming 团队，包括：

- **Turing** - 单元测试专家，负责编写测试方案和测试代码
- **Jobs** - 资深架构师，负责审查所有方案和代码
- **Thompson** - 生产代码专家，负责执行测试和编写实现代码

## 安装

技能文件位于 `~/.claude/skills/mob-programming/`，结构如下：

```
mob-programming/
├── SKILL.md           # 技能定义和主逻辑
├── README.md          # 本文档
└── agents/            # 团队成员角色定义
    ├── turing.md      # 单元测试专家 （向Alan Turing致敬）
    ├── jobs.md        # 架构师/审查者 （向Steve Jobs致敬）
    └── thompson.md    # 生产代码专家 （向Ken Thompson致敬）
```

## 使用方法

在 Claude Code 中输入：

```
/mob-programming
```

然后描述你要开发的功能：

```
实现一个用户认证模块，包含登录、注册、密码重置功能
```

## 结对编程

你可以使用“启动pair programming团队，完成……任务”来启动结对编程团队。该团队由两位agent组成，一位负责实现，一位负责审查。这个方式中，Job负责审查，Thompson负责实现。
'''

## 开发流程

```
任务拆解 → Turing 测试方案 → Jobs 审查 → Turing 测试代码 → Jobs 审查
    ↓
Thompson 执行测试 (RED) → 生产方案 → Jobs 审查 → 生产代码 → Jobs 审查
    ↓
子任务完成 → 继续下一任务
```

## 原则

- **TDD**: 先写测试，后写实现
- **SOLID**: 遵循 SOLID 设计原则
- **迭代审查**: 每个阶段都经过 Jobs 的审查
- **任务跟踪**: 使用 TaskCreate/TaskUpdate 跟踪进度

## 团队协作机制

- **主 Agent**: 负责任务拆解和流程协调
- **子 Agent**: 使用 Agent 工具启动，各司其职
- **通信**: 使用 SendMessage 工具在成员间传递消息
- **进度**: 使用 Task 系统跟踪每个子任务的状态

## 许可证

MIT
