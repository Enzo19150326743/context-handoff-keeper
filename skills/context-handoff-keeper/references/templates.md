# Context Handoff Keeper templates

Use these templates only when creating a document or when an existing structure is too weak. Remove empty optional fields. Do not rewrite a stronger document merely to match a template.

## Current handoff: `docs/handoffs/<topic>-current.md`

```md
# <Topic> current handoff

> Last updated: YYYY-MM-DD
> Topic: <topic>

## Continuation index

- Project context: `docs/project-context.md` (omit when absent)
- Related architecture: `docs/architecture/<topic>.md` (omit when absent)
- Primary entry points:

## Current state

- Completed:
- In progress:
- Not started:

## Session delta

- Added / changed / removed:
- Not implemented:

## Relevant files

- `path/to/file`: responsibility

## Decisions and constraints

- Preserve:
- Do not change:
- Rejected approaches and reasons:

## Verification and risk

- Verified:
- Unverified:
- Known issues / blockers:
- Open questions:

## Next step

- Priority task:
- Completion condition:
```

## Module architecture: `docs/architecture/<topic>.md`

```md
# <Topic> architecture

> Purpose: Record confirmed, stable module structure, entry points, and change boundaries.
> Last updated: YYYY-MM-DD

## Module overview

- Purpose:
- Primary entry points:

## File map

- `src/...`: responsibility

## Core structure

- Components / modules:
- Data flow / state flow / call chain:
- Design reasons:

## Change boundaries

- Safe extension points:
- Invariants:
```

## Project context: `docs/project-context.md`

```md
# Project context

> Purpose: Record confirmed project-wide facts, invariants, and primary entry points.
> Last updated: YYYY-MM-DD

## Project overview

- Purpose:
- Current stage:
- Technology stack and key dependencies:

## Primary entry points

- Run command:
- Build command:
- Deployment entry point:
- Main directories and responsibilities:
- Key files:

## Global invariants

-

## Durable document index

- `docs/architecture/...`
```

## Stage archive sequence

Do not use a second archive template. When the user explicitly asks to preserve a completed milestone:

1. Finalize the completed state in `docs/handoffs/<topic>-current.md` from evidence.
2. Copy it to `docs/handoffs/archive/YYYY-MM-DD-<topic>.md`.
3. Update the current handoff for the next stage using only confirmed scope.
4. If the archive path exists, add a user-confirmed milestone slug; never overwrite an archive.

Do not add archive snapshots to the durable-document index in `docs/project-context.md`.

