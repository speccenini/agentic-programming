# Guida: Inizializzazione Chat Codex (Frontend)

## Obiettivo
Avviare una nuova chat Codex frontend con contesto minimo ma completo, riducendo token e ambiguità.

## Pre-check (30 secondi)
Verifica che esistano:
- `docs/agent-handoffs/prompts/frontend-agent-prompt.md`
- `docs/agent-handoffs/TASK-START-CHECKLIST.md`
- `docs/agent-handoffs/registry.json`
- `docs/agent-handoffs/incoming/`
- `docs/agent-handoffs/outgoing/`
- `docs/shared/api-contract.md`

## Ordine consigliato dei messaggi in chat
1. Incolla `docs/agent-handoffs/prompts/frontend-agent-prompt.md`.
2. Incolla `docs/agent-handoffs/TASK-START-CHECKLIST.md`.
3. Dai il task specifico frontend in modo atomico.
4. Se esiste handoff in ingresso, incolla solo il file rilevante.

## Template task da incollare (frontend)
```md
Task frontend (atomico): <titolo breve>

Contesto:
- <2-4 punti max>

Input vincolanti:
- Handoff incoming: <filename o none>
- Decisioni: decision-000-ownership.md (+ eventuali altre)
- Contratto API (mirror note): docs/shared/api-contract.md

Output richiesto:
- Implementa solo nel frontend.
- Se serve lavoro backend, crea handoff in `docs/agent-handoffs/outgoing/` usando template standard.
- Aggiorna `docs/agent-handoffs/registry.json`.
- Risposta finale breve con file toccati e stato.
```

## Regole operative minime
- Non modificare backend.
- Non assumere cambi API non documentati.
- Se ambiguo: `Status: needs-clarification`.
- Se bloccato: `Status: blocked` con motivo sintetico.
- Se serve cambio contratto API: aprire handoff verso backend.

## Naming handoff
Usare sempre:
- `YYYY-MM-DD_HHMM_frontend-to-backend_<id>.md`

## Chiusura task (check rapido)
Prima di chiudere, verifica:
- `registry.json` aggiornato
- handoff outbound creato se necessario
- nessuna modifica backend
- output finale conciso

## Anti-pattern da evitare
- Messaggi lunghi narrativi.
- Task multi-feature nello stesso handoff.
- `implemented` senza allineare codice + docs + registry.
