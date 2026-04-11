# Guide: Codex Chat Initialization (Frontend)

## Goal
Start a new frontend Codex chat with minimal but complete context, reducing token usage and ambiguity.

## Pre-check (30 seconds)
Make sure these exist:
- `docs/agent-handoffs/prompts/frontend-agent-prompt.md`
- `docs/agent-handoffs/TASK-START-CHECKLIST.md`
- `docs/agent-handoffs/registry.json`
- `docs/agent-handoffs/incoming/`
- `docs/agent-handoffs/outgoing/`
- `docs/shared/api-contract.md`

## Recommended message order in chat
1. Paste `docs/agent-handoffs/prompts/frontend-agent-prompt.md`.
2. Paste `docs/agent-handoffs/TASK-START-CHECKLIST.md`.
3. Provide the frontend-specific task as an atomic request.
4. If there is an incoming handoff, paste only the relevant file.

## Task template to paste (frontend)
```md
Frontend task (atomic): <short title>

Context:
- <2-4 bullets max>

Required inputs:
- Incoming handoff: <filename or none>
- Decisions: decision-000-ownership.md (+ others if relevant)
- API contract (mirror notes): docs/shared/api-contract.md

Required output:
- Implement frontend-only changes.
- If backend work is needed, create a handoff in `docs/agent-handoffs/outgoing/` using the standard template.
- Update `docs/agent-handoffs/registry.json`.
- Provide a short final response with touched files and status.
```

## Minimal operating rules
- Do not modify backend.
- Do not assume undocumented API changes.
- If ambiguous: `Status: needs-clarification`.
- If blocked: `Status: blocked` with a short reason.
- If API contract changes are required: open a backend handoff.

## Handoff naming
Always use:
- `YYYY-MM-DD_HHMM_frontend-to-backend_<id>.md`

## Task closure (quick check)
Before closing, verify:
- `registry.json` updated
- outbound handoff created when needed
- no backend modifications
- concise final output

## Anti-patterns to avoid
- Long narrative messages.
- Multi-feature tasks in the same handoff.
- `implemented` without aligning code + docs + registry.
