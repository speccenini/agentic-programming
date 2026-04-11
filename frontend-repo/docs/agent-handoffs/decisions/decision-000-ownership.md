# decision-000-ownership

## Status
accepted

## Topic
Ownership boundaries between frontend and backend agents

## Context
Define clear ownership to reduce handoff loops and avoid duplicated or conflicting implementation.

## Decision
Ownership is defined as follows:

- Frontend owns UI, UX behavior, client-side validation, local state, and view components.
- Backend owns API behavior, server-side validation, auth enforcement, persistence, and execution logic.
- Backend is the source of truth for API contracts unless a shared decision states otherwise.
- Shared naming conventions must be documented in decision files.

## Rationale
- Reduces cross-repo ambiguity.
- Lowers back-and-forth clarifications.

## Consequences
### Positive
- Faster implementation flow.
- Clear accountability by boundary.

### Negative
- Some requests need explicit decisions before implementation.
- Requires consistent registry and decision maintenance.

## Applies to
- frontend
- backend
- shared contract

## Related handoffs
- <add related files when used>

## Notes
Initial governance decision.
