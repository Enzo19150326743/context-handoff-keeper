# Context Handoff Keeper

生成小而准确、以项目证据为基础的开发交接文档，让下一位编程 Agent 可以安全接手。

[English](README.md)

## 主要能力

- **聚焦接手：** 只记录继续工作所需的状态、文件、决策、风险和下一步。
- **长期事实门槛：** 不把临时、未确认、未验证或仅影响单个任务的信息写进架构和项目上下文。
- **清晰历史：** 默认复用一份当前 handoff；只有明确要求保留里程碑时才创建日期快照。
- **安全收集：** 不为了写交接额外启动测试或服务，也不把密钥和敏感原始日志复制进文档。
- **尊重项目约定：** 优先复用仓库现有文档结构，只有没有同类约定时才使用默认路径。

## 为什么需要这个 Skill

长时间的开发对话会混合已经完成的修改、未验证状态、临时方案和长期决定。普通摘要容易把这些状态混在一起，而完整聊天记录又太长，下一位 Agent 很难快速判断应该从哪里继续。

Context Handoff Keeper 会把当前任务状态和长期项目知识分开：当前 handoff 只负责“下一步从哪里继续”，长期文档只接收已经确认、跨任务仍然有效、影响模块或项目并且确有变化的事实。

## 安装

为 Codex 全局安装：

```powershell
npx skills add Enzo19150326743/context-handoff-keeper `
  --skill context-handoff-keeper `
  --agent codex --global --yes
```

为其他兼容 Agent 交互式安装：

```bash
npx skills add Enzo19150326743/context-handoff-keeper
```

> [!NOTE]
> GitHub 仓库公开后，上述命令才能从网络下载这个 Skill。

## 使用方法

显式调用：

```text
使用 $context-handoff-keeper 收口这轮开发，并准备好让下一位 agent 接手。
```

```text
使用 $context-handoff-keeper 更新项目上下文，但只写入这轮已经确认的稳定项目事实。
```

```text
使用 $context-handoff-keeper 保存这个已完成里程碑，然后为下一阶段更新当前 handoff。
```

当请求中出现 handoff、交接、收口、总结这轮、上下文过长、project context 或 architecture doc 等表达时，也可以自动触发。

## 写入模式

| 用户需求 | 处理结果 |
|---|---|
| 交接给下一位 Agent | 创建或更新当前 handoff |
| 更新架构或项目上下文 | 只写入通过稳定性门槛的事实 |
| 保存已完成里程碑 | 先归档已完成状态，再更新当前 handoff |
| 只要对话摘要，不要求写文件 | 只在对话中回复 |

只有仓库没有同类约定时，才使用以下默认路径：

```text
docs/
├── handoffs/
│   ├── <topic>-current.md
│   └── archive/YYYY-MM-DD-<topic>.md
├── architecture/<topic>.md
└── project-context.md
```

## 当前 handoff 会记录什么

- 下一位 Agent 应优先阅读的长期文档和主要入口
- 已完成、进行中和尚未开始的工作
- 本轮差异以及相关文件职责
- 决策、当前约束和已排除方案
- 已验证和未验证状态、风险、阻塞与待确认项
- 一个优先任务及其可判断的完成标准

handoff 会通过路径引用代码和长期文档，不复制完整 diff、长日志或大段实现内容。

## 安全边界

这个 Skill 会避免：

- 把计划、实验或未验证修改写成长期架构事实
- 为了填满模板而编造功能、业务规则或下一步范围
- 仅为了统一格式而重写已有文档
- 每轮开发都创建新的日期 handoff
- 仅为了生成文档而运行测试、构建、浏览器或服务
- 记录密钥、Token、Cookie、私钥、完整环境变量值、个人信息或未脱敏日志
- 在意图不明确时删除、移动或归档用户已有文档

## 仓库结构

```text
skills/context-handoff-keeper/
├── SKILL.md
├── agents/openai.yaml
└── references/templates.md
```

该 Skill 只有指令和模板，不需要运行时依赖。

