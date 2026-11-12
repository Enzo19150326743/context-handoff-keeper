> 本 Skill 的最新版本已迁移至 [Enzo的Skill](https://github.com/Enzo19150326743/Enzo-Skills)，请查看「Vibe Coding 必备 Skill」分类。本仓库归档保留历史版本。

# Context Handoff Keeper

**上下文窗口会满，一个功能也总会做完。可下一位编程 Agent 不该靠猜。**

Context Handoff Keeper 把“我上轮干了啥”整理成“你这轮先干啥”，并以仓库中的真实状态为依据，让新的编程 Agent 不必从头还原上一轮工作。

[English](README.md)

## 为按 Session 分块开发而生

它主要用在两个常见的开发边界：

- **上下文窗口快满了。** 趁这一轮对话还说得清，把当前真实状态从聊天上下文沉淀到项目里。
- **这一轮负责的功能做完了。** 收好本轮改动，标明哪些内容尚未验证，再给下一轮 Session 一个明确的起点。

最终得到的不是聊天记录，而是一份紧凑的继续入口：上一轮改了什么、还有什么没做或没验证、下一位 Agent 应该先读什么，以及这一轮优先做什么。

## 主要能力

- **上一轮说清楚：** 记录改了什么、做了哪些决定、还有哪些不确定。
- **下一轮有重点：** 给下一位编程 Agent 一个优先任务和可判断的完成标准。
- **长期事实门槛：** 不把临时、未确认、未验证或仅影响单个任务的信息写进架构和项目上下文。
- **清晰历史：** 默认复用一份当前 handoff；只有明确要求保留里程碑时才创建日期快照。
- **安全收集：** 不为了写交接额外启动测试或服务，也不把密钥和敏感原始日志复制进文档。
- **尊重项目约定：** 优先复用仓库现有文档结构，只有没有同类约定时才使用默认路径。

## 我为什么做这个 Skill

我最常用的开发方式，是尽量让一轮 Session 只负责一个功能。功能做完了，或者上下文窗口快满了，就换一个干净的新 Session；但上一轮的决定、未完成项和验证状态不能跟着聊天窗口一起丢掉。

一直拖着越来越长的对话继续开发，噪音会越来越多；直接开新 Session，又会让下一位 Agent 重新翻文件、猜决策。这个 Skill 解决的是中间那一步：把“上轮做了什么”和“下轮该做什么”写成一份小而准确的 handoff，同时只把已经确认、长期有效的事实同步进项目文档。

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

## 使用方法

显式调用：

```text
使用 $context-handoff-keeper 收口这轮开发。上下文窗口快满了，请记录本轮改了什么、哪些尚未验证，以及下一轮应该先做什么。
```

```text
使用 $context-handoff-keeper 收口这个已经完成的功能，并准备下一轮 Session 继续处理 <下一个功能>。
```

```text
使用 $context-handoff-keeper 保存这个已完成里程碑，然后为下一阶段更新当前 handoff。
```

```text
使用 $context-handoff-keeper 更新项目上下文，但只写入这轮已经确认的稳定项目事实。
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
