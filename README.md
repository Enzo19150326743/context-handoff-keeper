# Context Handoff Keeper

Create small, evidence-based development handoffs that another coding agent can continue safely.

[简体中文](README.zh-CN.md)

## Highlights

- **Focused continuation:** Record only the state, files, decisions, risks, and next step needed to resume work.
- **Durable context gate:** Keep tentative, unverified, and task-local information out of architecture and project-context documents.
- **Clean history:** Reuse one current handoff by default and create dated snapshots only for explicitly requested milestones.
- **Safe collection:** Never start tests or services just to write a handoff, and never copy secrets or raw sensitive logs into documentation.
- **Repository-aware:** Reuse existing documentation conventions before falling back to sensible default paths.

## Why this skill exists

Long development conversations accumulate decisions, partial changes, unverified work, and temporary plans. A generic summary often mixes those states together, while a full transcript is too noisy for the next agent.

Context Handoff Keeper separates current task state from durable project knowledge. It creates a compact continuation entry point and updates long-term documentation only when a fact is confirmed, stable, project-relevant, and genuinely new.

## Install

Install globally for Codex:

```powershell
npx skills add Enzo19150326743/context-handoff-keeper `
  --skill context-handoff-keeper `
  --agent codex --global --yes
```

Install interactively for any supported agent:

```bash
npx skills add Enzo19150326743/context-handoff-keeper
```

> [!NOTE]
> The GitHub repository must be public before these commands can download the skill.

## Use

Invoke the skill explicitly:

```text
Use $context-handoff-keeper to wrap up this development session and prepare the next agent to continue.
```

```text
Use $context-handoff-keeper to update project context, but only if this session introduced stable project-wide facts.
```

```text
Use $context-handoff-keeper to preserve this completed milestone, then prepare the current handoff for the next phase.
```

The skill can also trigger when a request mentions handoff, session wrap-up, context compression, project context, architecture documentation, `交接`, `收口`, or similar terms.

## Decision model

| Request | Result |
|---|---|
| Transfer work to another agent | Create or update the current handoff |
| Update architecture or project context | Write only facts that pass the stability gate |
| Preserve a completed milestone | Archive the completed state, then update the current handoff |
| Summarize without requesting files | Reply in the conversation only |

Default paths are used only when the repository has no equivalent convention:

```text
docs/
├── handoffs/
│   ├── <topic>-current.md
│   └── archive/YYYY-MM-DD-<topic>.md
├── architecture/<topic>.md
└── project-context.md
```

## What a handoff contains

A current handoff normally records:

- durable documents and primary entry points to read first
- completed, in-progress, and not-started work
- relevant file responsibilities and the session delta
- decisions, constraints, and rejected approaches
- verified and unverified status, risks, blockers, and open questions
- one priority next step with an observable completion condition

It links to code and durable documents instead of copying full diffs, long logs, or implementation bodies.

## Safeguards

The skill intentionally avoids:

- promoting plans, experiments, or unverified changes into long-term architecture
- inventing future features or business rules to fill a template
- rewriting documentation only for formatting consistency
- creating a new dated handoff for every session
- running tests, builds, browsers, or services solely for documentation
- recording secrets, tokens, cookies, private keys, full environment values, personal data, or unsanitized logs
- deleting, moving, or archiving existing user documentation without clear intent

## Repository layout

```text
skills/context-handoff-keeper/
├── SKILL.md
├── agents/openai.yaml
└── references/templates.md
```

The skill is instruction-only and has no runtime dependencies.

