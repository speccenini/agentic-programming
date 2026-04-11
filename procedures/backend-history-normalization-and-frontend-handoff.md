# Procedure: Backend History Normalization + Frontend Handoff

## Goal
Normalize legacy backend markdown handoffs into the standard format and generate a new frontend handoff when needed.

## Scope
- Repository: backend
- Involved folders:
  - `docs/agent-handoffs/incoming/`
  - `docs/agent-handoffs/outgoing/`
  - `docs/agent-handoffs/templates/`
  - `docs/agent-handoffs/registry.json`

## Prerequisites
Read first:
- `docs/agent-handoffs/decisions/decision-000-ownership.md`
- `docs/agent-handoffs/FILENAME-CONVENTION.md`
- `docs/agent-handoffs/templates/TEMPLATE-handoff.md`
- `docs/agent-handoffs/registry.json`
- `openapi.yaml` (if API-related)

## Priority rule (token-efficient)
Normalize first:
1. files with status: `requested`, `blocked`, `needs-clarification`
2. recent and still relevant files
3. files impacting API/contract

Avoid normalizing the entire history unless necessary.

## Operational steps
1. Identify legacy backend files to normalize.
2. For each legacy file:
- keep the functional meaning unchanged
- rewrite using `TEMPLATE-handoff.md`
- use standard filename: `YYYY-MM-DD_HHMM_<direction>_<id>.md`
- add in `Notes`: `Legacy source: <old-filename>.md`
3. Save the normalized file in `docs/agent-handoffs/outgoing/` or `incoming/` based on direction.
4. Update `docs/agent-handoffs/registry.json` with id, direction, topic, status, and date.
5. If content requires frontend work:
- create a new backend->frontend handoff in `docs/agent-handoffs/outgoing/`
- include `Files impacted`, `Validation`, `Done when`
- reference `openapi.yaml` if the contract changed
6. Manually copy the file to frontend `incoming/`, preserving the filename.

## Quick request template for Codex (backend)
```md
Normalize legacy backend handoff files in `docs/agent-handoffs/` using:
- `docs/agent-handoffs/templates/TEMPLATE-handoff.md`
- `docs/agent-handoffs/FILENAME-CONVENTION.md`

Rules:
- Do not change functional meaning.
- Rename with format: `YYYY-MM-DD_HHMM_<direction>_<id>.md`.
- Update `docs/agent-handoffs/registry.json`.
- If frontend work emerges, generate a new handoff in `docs/agent-handoffs/outgoing/`.
- Include in `Notes`: `Legacy source: <old-file>.md`.
```

## Done criteria
- Priority legacy files are normalized in standard format.
- `registry.json` is updated and consistent.
- Any required backend->frontend handoffs are created and ready for manual copy.
