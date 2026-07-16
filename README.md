# Context Handoff Keeper

**Context windows fill up. Features finish. The next coding agent should not have to guess what happened.**

Context Handoff Keeper turns “what I did last session” into “what the next session should do,” grounded in the repository so a fresh coding agent can continue without reconstructing the work from scratch.

[简体中文](README.zh-CN.md)

## Built for session-sized development

Use it at two common development boundaries:

- **The context window is getting full.** Capture the current truth before a long conversation becomes the only place that remembers it.
- **One feature-sized session is complete.** Close out what changed, record what is still unverified, and give the next session one clear starting task.

The result is not a transcript. It is a compact continuation point: what the previous session changed, what remains unfinished or unverified, what the next agent should read first, and what it should do next.

## Highlights

- **Previous session, captured:** Record what changed, what was decided, and what remains uncertain.
- **Next session, focused:** Give the next coding agent one priority task and an observable completion condition.
- **Durable context gate:** Keep tentative, unverified, and task-local information out of architecture and project-context documents.
- **Clean history:** Reuse one current handoff by default and create dated snapshots only for explicitly requested milestones.
- **Safe collection:** Never start tests or services just to write a handoff, and never copy secrets or raw sensitive logs into documentation.
- **Repository-aware:** Reuse existing documentation conventions before falling back to sensible default paths.

## Why I built this

I prefer to give each coding session one focused feature when possible. When that feature is done—or the context window is nearly full—I want to start a fresh session without losing the decisions, unfinished work, and verification status from the previous one.

Dragging an overloaded conversation forward is noisy. Starting a new session with no handoff makes the next agent rediscover the same files and decisions. This skill creates the middle path: a short, evidence-based handoff that says what happened last session and what should happen next, while promoting only confirmed, durable facts into long-term project documentation.

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
Use $context-handoff-keeper because this context window is getting full. Record what changed, what is still unverified, and what the next session should do first.
```

```text
Use $context-handoff-keeper to close out this completed feature and prepare the next session to work on <next-feature>.
```

```text
Use $context-handoff-keeper to preserve this completed milestone, then prepare the current handoff for the next phase.
```

```text
Use $context-handoff-keeper to update project context, but only if this session introduced stable project-wide facts.
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
