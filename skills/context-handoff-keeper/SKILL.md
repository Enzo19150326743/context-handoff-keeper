---
name: context-handoff-keeper
description: Use when the user explicitly asks to wrap up a development session, create or update a handoff, transfer work to another agent, summarize changes for continuation, reduce conversation context, preserve a milestone snapshot, or update project context or architecture documentation; also trigger on "handoff", "context handoff", "project context", "architecture doc", "交接", "收口", "总结这轮", or "上下文过长".
---

# Context Handoff Keeper

## Core principle

Create the smallest evidence-based continuation package that lets the next agent resume safely.

Treat a handoff as the current continuation point, not a permanent activity log. Put durable facts in architecture or project-context documents only after they pass the stability gate. Reuse the current handoff by default; create dated history only when the user explicitly asks to preserve a milestone or phase.

## Choose the smallest mode

| Mode | Use when | Write behavior |
|---|---|---|
| Current handoff | Wrap up, switch conversations, or transfer to another agent | Create or update the current handoff |
| Long-term sync | Update project context, architecture, or another durable reference | Update only stable facts; do not require a handoff |
| Stage archive | Preserve a completed milestone or phase history | Archive the completed current state, then update the current handoff for what comes next |
| Conversation summary | Summarize without asking for files | Reply in the conversation only |

If the request combines modes, evaluate each write target independently. When intent is ambiguous, write less. Ask only when different interpretations would materially change the result.

## Find the repository and existing conventions

Determine the real project root before writing:

1. Prefer the repository containing the files changed in the current task.
2. In a multi-repository workspace, limit the handoff to the repository involved in the request.
3. If no single repository can be identified, ask instead of writing into the terminal working directory.

Reuse an existing `HANDOFF.md`, handoff directory, context directory, architecture directory, or equivalent convention. Only use these defaults when no equivalent exists:

- `docs/handoffs/<topic>-current.md` for the current continuation point
- `docs/handoffs/archive/YYYY-MM-DD-<topic>.md` for an explicitly requested archive
- `docs/architecture/<topic>.md` for stable module facts
- `docs/project-context.md` for stable project-wide facts

Derive `topic` from the main feature or module involved in the task. Use English kebab-case file names and reuse an existing synonymous topic when possible.

## Collect only necessary evidence

Read only what is needed:

- confirmed goals, completed work, constraints, and remaining work from the conversation
- code, configuration, and documentation involved in the task
- current branch, Git status, and relevant diffs when Git applies
- related long-term documents and the current handoff for the same topic

Classify every material fact:

- **Confirmed:** present in code, configuration, documentation, or an explicit user decision
- **Unverified:** implemented or changed, but not verified in this session
- **Planned:** not implemented, disputed, tentative, or likely to change

Do not run tests, builds, browsers, or services merely to create a handoff. Record only checks that actually occurred. Never include secrets, tokens, cookies, private keys, full environment-variable values, personal information, or unsanitized logs. Mention variable names or redacted conclusions only when relevant.

## Apply the stability gate

A fact may enter a long-term document only when all conditions are true:

1. Code, configuration, documentation, or an explicit user decision confirms it.
2. It will remain true after the next task is completed.
3. It affects the module or project rather than one task.
4. It adds or changes information not already recorded accurately.

Keep unimplemented, unverified, experimental, disputed, or temporary information out of long-term documents. Put it in the current handoff and label its status. Do not add a "future architecture" or "pending decision" section to a durable architecture document merely to preserve a plan.

| Information | Destination |
|---|---|
| Current progress, local differences, unfinished work, temporary constraints | Current handoff |
| Stable component boundaries, data flow, call chains, API contracts, schemas, module invariants | `docs/architecture/<topic>.md` |
| Stable stack, key dependencies, run/build/deploy entry points, global layers, security or collaboration invariants | `docs/project-context.md` |
| Unconfirmed, unimplemented, unverified, or disputed information | Current handoff only, with status |

Do not promote ordinary bug fixes, small visual changes, copy edits, or one-session completion lists to long-term documentation.

## Execute incrementally

1. Identify the mode, repository root, and existing document convention.
2. Collect evidence from the smallest relevant set of sources.
3. Classify facts as confirmed, unverified, planned, module-stable, or project-stable.
4. Select the minimum write targets that pass their respective gates.
5. Edit only necessary sections and preserve user-written content and structure.
6. Skip no-op rewrites; do not change dates or wording when the information is unchanged.
7. Report only files actually created or modified and the continuation entry point.

For a stage archive:

1. Finalize the completed stage in the current handoff using available evidence.
2. Copy that completed state to `docs/handoffs/archive/YYYY-MM-DD-<topic>.md`.
3. Update the current handoff for the next stage using only user-confirmed or repository-supported scope.
4. Never overwrite an existing archive. Add a user-confirmed milestone slug when needed to make the path unique.

Do not invent future features, business rules, implementation steps, or completion claims to fill a template.

## Current handoff requirements

Make the next agent able to see:

- continuation index: durable documents and primary entry points to read first
- current state: completed, in progress, and not started
- session delta: what changed from the previous valid state
- relevant files: only files needed to continue, with responsibilities
- decisions and constraints: preserve, avoid changing, rejected approaches, and reasons
- verification and risk: verified, unverified, known issues, blockers, and open questions
- next step: one priority task with an observable completion condition

Include branch, commit, and uncommitted-change information only when relevant. Link to code and documents instead of copying code, full diffs, long logs, or long-term document bodies. Prefer 80 lines or fewer, but never remove blockers, risks, or unverified status solely to meet a line target.

When one session covers unrelated features, keep one current handoff with clearly separated sections and one overall priority. Split handoffs only when the user explicitly requests parallel ownership.

## Long-term document requirements

Architecture documents contain stable module purpose, entry points, file responsibilities, data or call flow, design reasons, extension points, and invariants. They do not contain open questions or session history.

Project context contains stable project purpose and stage, technology stack, key dependencies, run/build/deploy entry points, directory responsibilities, global invariants, and an index of durable documents. It does not index every handoff.

When a durable document already states a fact accurately, do not edit it. When a fact changes, replace or remove the stale statement instead of appending a contradictory version. Store each durable fact in one authoritative document and link to it elsewhere.

## Language and final response

Match the user's requested language. Otherwise follow the dominant language of existing project documentation; if no signal exists, use English. Keep file names in English kebab-case.

Describe constraints neutrally as current constraints, do-not-change boundaries, rejected approaches, or open questions. Do not narrate them as user prohibitions.

Do not paste the handoff body into the final response. Report:

1. **Result:** the mode used and whether the work produced a material change.
2. **Updated files:** only files actually created or modified; when nothing changed, name the reused entry point.
3. **Next-session prompt:** only after creating or updating a handoff, normally no more than six lines.

Suggested prompt:

```text
Continue <topic>. Read docs/handoffs/<topic>-current.md first, then follow its continuation index.
Do not infer state from prior conversation memory; use the repository and documents as the source of truth.
Handle only the priority task named in the handoff and close it against the stated completion condition.
```

## Templates

Read `references/templates.md` only when creating a document or when the existing structure is too weak to support continuation. Treat templates as minimal scaffolding, not mandatory forms. Preserve a stronger existing structure.

