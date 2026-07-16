# Context Handoff Keeper 开源发布设计

## 目标

把本机正在使用的 `context-handoff-keeper` 整理为一个可公开审阅、可通过 GitHub 安装、兼容开放 Agent Skills 规范的开源 Skill，同时不改动当前全局安装版本，也不在用户最终确认前创建远程仓库。

## 已批准方案

- 仓库名使用 `context-handoff-keeper`。
- 第一版发布纯 Agent Skill，不附带 MCP、脚本或 Codex 插件。
- 采用 MIT 许可证。
- `README.md` 使用英文，`README.zh-CN.md` 提供中文说明。
- Skill 指令使用英文，触发描述覆盖英文和中文常见表达。
- 生成的 handoff 和长期文档跟随用户或仓库的主要语言，不再强制简体中文。
- 仓库使用 `skills/context-handoff-keeper/` 布局，便于开放 Skills CLI 自动发现。

## 目录设计

```text
context-handoff-keeper/
├── README.md
├── README.zh-CN.md
├── LICENSE
├── docs/plans/
└── skills/context-handoff-keeper/
    ├── SKILL.md
    ├── agents/openai.yaml
    └── references/templates.md
```

## 行为边界

- 只在用户明确要求总结、handoff、交接、上下文压缩或长期项目文档同步时触发。
- 优先复用已有 handoff 和项目文档约定。
- 临时进度进入当前 handoff；只有稳定、已确认、跨任务有效的事实才能进入长期文档。
- 不为了生成 handoff 启动测试、浏览器、服务或构建。
- 不记录密钥、Token、Cookie、完整环境变量值、个人信息或未脱敏日志。
- 不自动删除、移动或归档用户已有文档。

## 质量与发布门槛

- `SKILL.md` 的 `name` 与目录名一致，描述不超过规范上限。
- 引用文件使用相对路径，主说明保持在 500 行以内。
- 结构校验通过。
- Skills CLI 能发现并安装 Skill 到隔离临时目录。
- 公开上传前再次展示文件清单、Git 状态和安装命令，并等待用户明确确认。

