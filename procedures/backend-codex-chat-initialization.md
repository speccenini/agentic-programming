# Guida: Inizializzazione Chat Codex (Backend)

## Obiettivo
Avviare una nuova chat Codex backend con contesto minimo ma completo, riducendo token e ambiguità.

## Pre-check (30 secondi)
Verifica che esistano:
- `docs/agent-handoffs/prompts/backend-agent-prompt.md`
- `docs/agent-handoffs/TASK-START-CHECKLIST.md`
- `docs/agent-handoffs/registry.json`
- `docs/agent-handoffs/incoming/`
- `docs/agent-handoffs/outgoing/`
- `openapi.yaml`

## Ordine consigliato dei messaggi in chat
1. Incolla `docs/agent-handoffs/prompts/backend-agent-prompt.md`.
2. Incolla `docs/agent-handoffs/TASK-START-CHECKLIST.md`.
3. Dai il task specifico backend in modo atomico.
4. Se esiste handoff in ingresso, incolla solo il file rilevante.

## Template task da incollare (backend)
```md
Task backend (atomico): <titolo breve>

Contesto:
- <2-4 punti max>

Input vincolanti:
- Handoff incoming: <filename o none>
- Decisioni: decision-000-ownership.md (+ eventuali altre)
- Contratto API: openapi.yaml

Output richiesto:
- Implementa solo nel backend.
- Se serve lavoro frontend, crea handoff in `docs/agent-handoffs/outgoing/` usando template standard.
- Aggiorna `docs/agent-handoffs/registry.json`.
- Risposta finale breve con file toccati e stato.
```

## Regole operative minime
- Non modificare frontend.
- Non inventare campi API non documentati.
- Se ambiguo: `Status: needs-clarification`.
- Se bloccato: `Status: blocked` con motivo sintetico.
- Se cambia contratto API: aggiornare `openapi.yaml`.

## Naming handoff
Usare sempre:
- `YYYY-MM-DD_HHMM_backend-to-frontend_<id>.md`

## Chiusura task (check rapido)
Prima di chiudere, verifica:
- `registry.json` aggiornato
- handoff outbound creato se necessario
- `openapi.yaml` aggiornato se contratto cambiato
- output finale conciso

## Anti-pattern da evitare
- Messaggi lunghi narrativi.
- Task multi-feature nello stesso handoff.
- `implemented` senza allineare codice + docs + registry.
