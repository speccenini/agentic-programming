# Filename Convention

Use this format for all handoff files:

`YYYY-MM-DD_HHMM_<direction>_<id>.md`

Examples:
- `2026-04-11_1420_frontend-to-backend_004.md`
- `2026-04-11_1435_backend-to-frontend_007.md`

Rules:
- `YYYY-MM-DD`: local date.
- `HHMM`: 24h local time, zero-padded.
- `<direction>`: `frontend-to-backend` or `backend-to-frontend`.
- `<id>`: 3 digits, per direction (`001`, `002`, ...).
- Never rename files after handoff.
- Copy outgoing file to the other repo incoming folder preserving filename.
