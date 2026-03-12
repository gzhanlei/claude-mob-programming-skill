# Claude Mob Programming Skill

一个用于 Claude Code 的 Mob Programming（团队编程）技能，支持 Pair Programming 和 Mob Programming 模式，自动组建专家团队进行协作开发。

## 目的

解决单人开发的局限性，通过模拟团队协作模式：
- **专业分工**：测试专家、实现专家、架构师各司其职
- **质量保证**：自动代码审查，确保符合 TDD 和 SOLID 原则
- **效率提升**：并行工作，减少上下文切换
- **知识传递**：通过协作过程学习最佳实践

## 原理

### 角色定义

| 角色 | 职责 | 工作方式 |
|------|------|----------|
| **Cunningham** | 测试专家 / Navigator | 分析代码、设计测试方案、审查代码质量 |
| **Thompson** | 实现专家 / Driver | 编写测试代码和生产代码 |
| **Jobs** | 架构师 / 审查者 | 代码审查、质量把关、架构决策 |

### 工作流程

**TDD 模式：**
```
Cunningham(测试方案) → Jobs(审查) → Thompson(实现) → Cunningham(验证)
```

**Pair 覆盖率模式：**
```
Cunningham(Navigator) 设计方案 → Thompson(Driver) 编写代码 → Cunningham 审查
```

## 使用说明

### 触发方式

在 Claude Code 中输入以下任一指令：

```
"pair programming" / "结对编程" / "结对"
"mob programming" / "团队编程"
"代码审查" / "帮我重构"
"提升测试覆盖率" / "测试覆盖率"
提到 "Cunningham" / "Jobs" / "Thompson"
```

### 团队模式选择

| 模式 | 团队配置 | 适用场景 |
|------|----------|----------|
| **Pair TDD 模式** | Cunningham + Thompson | 新功能开发 |
| **Pair 审查模式** | Jobs + Thompson | 代码重构/审查 |
| **Pair 覆盖率模式** | Cunningham(Navigator) + Thompson(Driver) | 提升测试覆盖率 |
| **Mob 三人模式** | Cunningham + Jobs + Thompson | 复杂架构设计 |

### 使用流程

1. **描述任务**：向 Claude 描述你要开发的功能
2. **确认团队配置**：Claude 会展示推荐的团队配置，确认后启动
3. **自动协作**：团队成员自动分配任务、编写代码、互相审查
4. **进度跟踪**：通过 TaskList 实时查看任务状态
5. **结果验收**：任务完成后，Claude 会汇报最终成果

### 示例

**示例 1：新功能开发**
```
用户：帮我用 TDD 实现一个用户注册功能
→ 启动 Pair TDD 模式
→ Cunningham 设计测试方案
→ Thompson 编写测试和生产代码
→ 自动运行测试验证
```

**示例 2：提升测试覆盖率**
```
用户：提升 payment 模块测试覆盖率到 90%
→ 启动 Pair 覆盖率模式
→ Cunningham 分析代码并设计测试方案
→ Thompson 编写测试代码
→ 循环直到覆盖率达到目标
```

## 安装

1. 确保你正在使用 Claude Code
2. 将此 skill 添加到你的 `.claude/skills/` 目录：
   ```bash
   git clone https://github.com/gzhanlei/claude-mob-programming-skill.git
   cp claude-mob-programming-skill/SKILL.md ~/.claude/skills/mob-programming/SKILL.md
   ```
3. 重启 Claude Code 或等待 skill 自动加载

## 配置

### 角色定义文件

可以在 `~/.claude/agents/` 目录下自定义角色：
- `cunningham.md` - 测试专家角色定义
- `thompson.md` - 实现专家角色定义
- `jobs.md` - 架构师角色定义

### 团队纪律

- **Navigator 禁止编写代码**：只能分析、设计、审查
- **Driver 禁止做架构决策**：只能按照 Navigator 的方案执行
- **必须通知 Team Lead**：任务完成时必须通知
- **任务列表是真相源**：所有状态变更必须通过 TaskUpdate

## 核心特性

### 1. 智能任务分配

自动根据任务类型选择最佳团队配置：
- 新功能开发 → Pair TDD
- 代码重构 → Pair 审查
- 提升覆盖率 → Pair 覆盖率
- 复杂架构 → Mob 三人

### 2. 自动错误恢复

当 Agent 无响应时，系统会：
1. 尝试唤醒 Agent
2. 检查任务文件状态
3. 自动重启无响应 Agent
4. 必要时降级到手动模式

### 3. 角色边界强制执行

通过硬性规定确保角色边界：
- Navigator 绝不动手编写代码
- Driver 绝不擅自做架构决策
- 违规者会被警告或移出团队

### 4. 任务通知机制

完成任务时必须：
1. 更新任务状态（TaskUpdate）
2. 通知 Team Lead（SendMessage）
3. 验证任务列表状态（TaskList）

## 故障排除

### Agent 无响应

- 系统会自动尝试唤醒和重启 Agent
- 如果持续失败，会降级到手动协调模式

### 权限被拒绝

- 首次使用时会请求 Agent 权限
- 拒绝后将使用手动协调模式（单 Agent 完成所有工作）

### 任务状态不一致

- 确保每次任务分配都使用 TaskUpdate
- 定期使用 TaskList 查看真实状态
- 不要依赖 SendMessage 作为状态记录

## 实际案例

### 案例 1：Payment 模块覆盖率提升

**背景**：Payment 模块覆盖率 19.4%，目标是 90%

**过程**：
1. 启动 Pair 覆盖率模式（Cunningham + Thompson）
2. 分 6 个 Phase 逐步提升：
   - Phase 1: 接口定义
   - Phase 2: ProcessWechatNotify/ProcessAlipayNotify 测试
   - Phase 3: CreateRefund/CleanExpiredPayments 测试
   - Phase 4: Storage 层测试
   - Phase 5: Handler 层测试
   - Phase 6: 补充 Service 层测试
3. 最终覆盖率：**82.2%**（从 19.4% 提升 +62.8%）

**经验**：
- 通过 TaskList 跟踪每个 Phase 的进度
- Cunningham 严格遵守 Navigator 职责，只指导不编码
- Thompson 作为 Driver 高效执行所有代码编写

## 贡献

欢迎提交 Issue 和 PR 改进此 skill。

### 改进方向

- 支持更多团队配置模式
- 优化 Agent 通信机制
- 增加更多角色定义模板
- 改进错误恢复机制

## 许可证

MIT License

---

**注意**：此 skill 需要 Claude Code 环境支持 Agent 和 Team 工具。完整技能定义请查看 `SKILL.md` 文件。
