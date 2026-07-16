# Context Handoff Keeper Open Source Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 将现有中文个人 Skill 整理成可公开安装、双语说明、符合开放 Agent Skills 结构的本地 Git 仓库。

**Architecture:** 仓库根目录承载面向人的 README、许可证和设计记录；实际可安装内容放在 `skills/context-handoff-keeper/`。保持 Skill 为纯指令型，模板按需加载，不引入脚本或外部依赖。

**Tech Stack:** Markdown、YAML、Git、GitHub CLI、Skills CLI、Python Skill 校验脚本。

---

### Task 1: 建立行为基线

**Files:**
- Modify: `docs/plans/2026-07-16-context-handoff-keeper-open-source.md`

**Step 1:** 准备三个不加载 Skill 的场景：当前交接、仅长期同步、阶段存档。

**Step 2:** 记录基线输出中遗漏的状态区分、安全边界和文档写入边界。

**Step 3:** 把观察结果用于约束国际化改写，不把测试特例写成叙事。

**Baseline observations:**

- 当前交接场景基本能区分“代码已落地”和“尚未验证”，但缺少稳定的默认路径规则，并加入了未从项目证据得出的安全检查项。
- 长期同步场景把尚未决定、未完成、未验证的 PostgreSQL 迁移写入架构文档；新版必须明确禁止把此类计划中信息写入长期文档。
- 阶段存档场景能先保留已完成阶段，但自行扩写了退款业务规则和实现步骤；新版必须要求从现有文件和用户决定提取内容，不得编造下一阶段范围。
- 阶段存档应先保存已完成阶段，再把当前 handoff 更新为下一阶段，避免归档文件错误记录未来状态。

### Task 2: 创建开源 Skill 内容

**Files:**
- Create: `skills/context-handoff-keeper/SKILL.md`
- Create: `skills/context-handoff-keeper/agents/openai.yaml`
- Create: `skills/context-handoff-keeper/references/templates.md`

**Step 1:** 复制现有功能边界，并将核心指令改写为简洁英文。

**Step 2:** 让输出语言跟随用户或仓库语言，保留中英文触发词。

**Step 3:** 保持模板为按需引用资源，不新增脚本和依赖。

### Task 3: 编写公开仓库说明

**Files:**
- Create: `README.md`
- Create: `README.zh-CN.md`
- Create: `LICENSE`

**Step 1:** 在英文 README 中说明用途、边界、安装方式、使用示例和目录结构。

**Step 2:** 提供内容一致的中文 README，并让两份文档相互链接。

**Step 3:** 添加以 GitHub 用户名作为版权人的 MIT 许可证。

### Task 4: 行为复测与收紧

**Files:**
- Modify when needed: `skills/context-handoff-keeper/SKILL.md`
- Modify when needed: `skills/context-handoff-keeper/references/templates.md`

**Step 1:** 使用相同三个场景加载新版 Skill。

**Step 2:** 检查模式选择、稳定性门槛、安全边界和下一步提示是否符合设计。

**Step 3:** 只修复复测暴露的缺口，避免扩张功能。

### Task 5: 结构与隔离安装检查

**Files:**
- Test: `skills/context-handoff-keeper/SKILL.md`
- Test: `skills/context-handoff-keeper/agents/openai.yaml`
- Test: `skills/context-handoff-keeper/references/templates.md`

**Step 1:** 运行 Skill 结构校验。

```powershell
python "$env:CODEX_HOME\skills\.system\skill-creator\scripts\quick_validate.py" skills/context-handoff-keeper
```

Expected: 输出有效 Skill 的成功结果。

**Step 2:** 使用 Skills CLI 列出仓库中的可安装 Skill。

```powershell
npx --yes skills@latest add . --list
```

Expected: 列出 `context-handoff-keeper`。

**Step 3:** 在隔离临时目录执行项目级复制安装。

Expected: 临时目录中生成 Codex Skill，且不修改全局安装和发布仓库。

### Task 6: 本地交付并等待发布确认

**Files:**
- Review: all tracked files

**Step 1:** 检查 Git 差异、未跟踪文件和敏感信息扫描结果。

**Step 2:** 创建本地提交，不配置远程地址、不调用 `gh repo create`、不执行 `git push`。

**Step 3:** 向用户报告待公开文件、安装命令和远程仓库参数，等待“确认公开发布”。

