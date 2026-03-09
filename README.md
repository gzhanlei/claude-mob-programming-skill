# Claude Mob Programming Skill

[![GitHub](https://img.shields.io/badge/GitHub-Public-blue)](https://github.com/gzhanlei/claude-mob-programming-skill)

一个用于 [Claude Code](https://claude.ai/code) 的 Mob Programming Skill，组建三人开发团队进行 TDD（测试驱动开发）。

## 简介

这个 skill 创建一个虚拟的 Mob Programming 团队，包括：

- **Turing** - 单元测试专家，负责编写测试方案和测试代码
- **Jobs** - 资深架构师，负责审查所有方案和代码
- **Thompson** - 生产代码专家，负责编写实现代码

## 安装

1. 将全部文件复制到 Claude Code 的 skills 的对应子目录：
   ```bash
   mkdir -p ~/.claude/skills/mob-programming
   cp * ~/.claude/skills/mob-programming/
   ```

2. 重启 Claude Code 或在设置中刷新 skills

## 使用方法

在 Claude Code 中使用以下命令：

```
/mob-programming
```

然后描述你要开发的功能：

```
实现一个用户认证模块，包含登录、注册、密码重置功能
```

## 开发流程

```
Turing (写测试方案) → Jobs (审查) → Turing (写测试代码) → Jobs (审查) 
→ Thompson (执行测试/写生产代码) → Jobs (审查) → 完成 ✅
```

## 原则

- **TDD**: 先写测试，后写实现
- **SOLID**: 遵循 SOLID 设计原则
- **迭代审查**: 每个阶段都经过 Jobs 的严格审查
- **静默等待**: 团队成员通过 SendMessage 通信，无需轮询

## 许可证

MIT
