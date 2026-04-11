# Procedura: Normalizzazione Storico Backend + Handoff per Frontend

## Obiettivo
Normalizzare i vecchi handoff markdown del backend nel formato standard e generare, quando serve, un nuovo handoff verso frontend.

## Scope
- Repository: backend
- Cartelle coinvolte:
  - `docs/agent-handoffs/incoming/`
  - `docs/agent-handoffs/outgoing/`
  - `docs/agent-handoffs/templates/`
  - `docs/agent-handoffs/registry.json`

## Prerequisiti
Leggere prima:
- `docs/agent-handoffs/decisions/decision-000-ownership.md`
- `docs/agent-handoffs/FILENAME-CONVENTION.md`
- `docs/agent-handoffs/templates/TEMPLATE-handoff.md`
- `docs/agent-handoffs/registry.json`
- `openapi.yaml` (se il cambio è API-related)

## Regola di priorità (token-efficient)
Normalizzare prima:
1. file con `status`: `requested`, `blocked`, `needs-clarification`
2. file più recenti e ancora rilevanti
3. file che impattano API/contratto

Evitare di normalizzare tutto lo storico se non necessario.

## Passi operativi
1. Identificare i file legacy backend da normalizzare.
2. Per ogni file legacy:
- mantenere invariato il significato funzionale
- riscrivere nel formato `TEMPLATE-handoff.md`
- usare filename standard: `YYYY-MM-DD_HHMM_<direction>_<id>.md`
- aggiungere in `Notes`: `Legacy source: <old-filename>.md`
3. Salvare il file normalizzato in `docs/agent-handoffs/outgoing/` o `incoming/` in base alla direzione.
4. Aggiornare `docs/agent-handoffs/registry.json` con id, direzione, topic, stato, data.
5. Se il contenuto richiede lavoro frontend:
- creare nuovo handoff backend->frontend in `docs/agent-handoffs/outgoing/`
- includere `Files impacted`, `Validation`, `Done when`
- referenziare `openapi.yaml` se il contratto è cambiato
6. Copiare manualmente il file in `incoming/` del frontend mantenendo nome invariato.

## Template rapido per richiesta a Codex (backend)
```md
Normalizza i file handoff legacy backend in `docs/agent-handoffs/` secondo:
- `docs/agent-handoffs/templates/TEMPLATE-handoff.md`
- `docs/agent-handoffs/FILENAME-CONVENTION.md`

Regole:
- Non cambiare il significato funzionale.
- Rinominare con formato: `YYYY-MM-DD_HHMM_<direction>_<id>.md`.
- Aggiornare `docs/agent-handoffs/registry.json`.
- Se emerge lavoro lato frontend, genera un nuovo handoff in `docs/agent-handoffs/outgoing/`.
- Includi in `Notes`: `Legacy source: <vecchio-file>.md`.
```

## Criterio di done
- I file legacy prioritari sono normalizzati nel formato standard.
- `registry.json` è aggiornato e coerente.
- Eventuali handoff backend->frontend necessari sono creati e pronti per copia manuale.
