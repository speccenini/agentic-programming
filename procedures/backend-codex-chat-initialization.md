# Guide: Codex Chat Initialization (Backend)

## Goal
Start a new backend Codex chat with minimal but complete context, reducing token usage and ambiguity.

## Pre-check (30 seconds)
Make sure these exist:
- `docs/agent-handoffs/prompts/backend-agent-prompt.md`
- `docs/agent-handoffs/TASK-START-CHECKLIST.md`
- `docs/agent-handoffs/registry.json`
- `docs/agent-handoffs/incoming/`
- `docs/agent-handoffs/outgoing/`
- `openapi.yaml`

## Recommended message order in chat
1. Paste `docs/agent-handoffs/prompts/backend-agent-prompt.md`.
2. Paste `docs/agent-handoffs/TASK-START-CHECKLIST.md`.
3. Provide the backend-specific task as an atomic request.
4. If there is an incoming handoff, paste only the relevant file.

## Task template to paste (backend)
```md
Backend task (atomic): <short title>

Context:
- <2-4 bullets max>

Required inputs:
- Incoming handoff: <filename or none>
- Decisions: decision-000-ownership.md (+ others if relevant)
- API contract: openapi.yaml

Required output:
- Implement backend-only changes.
- If frontend work is needed, create a handoff in `docs/agent-handoffs/outgoing/` using the standard template.
- Update `docs/agent-handoffs/registry.json`.
- Provide a short final response with touched files and status.
```

## Minimal operating rules
- Do not modify frontend.
- Do not invent undocumented API fields.
- If ambiguous: `Status: needs-clarification`.
- If blocked: `Status: blocked` with a short reason.
- If API contract changes: update `openapi.yaml`.

## Handoff naming
Always use:
- `YYYY-MM-DD_HHMM_backend-to-frontend_<id>.md`

## Task closure (quick check)
Before closing, verify:
- `registry.json` updated
- outbound handoff created when needed
- `openapi.yaml` updated if contract changed
- concise final output

## Anti-patterns to avoid
- Long narrative messages.
- Multi-feature tasks in the same handoff.
- `implemented` without aligning code + docs + registry.
