# Mock use-cases from contract pack

## 1) Business producer flow (NODES -> VTN)
1. Obtain admin/business token (`00_auth_token.*`).
2. Create program (`01_program_create.*`).
3. Create event in program (`02_event_create.*`).
4. Optionally cancel event (`06_event_cancel.*`).

## 2) VEN polling and response flow
1. VEN token acquisition (`00_auth_token.*` with VEN credentials).
2. Poll events (`03_ven_poll_events.*`).
3. Submit opt-in/opt-out as report payload (`04_event_optin.*`).
4. Submit telemetry/report updates (`05_reports_post.*`).

## Determinism rules for mocks
- Replace dynamic IDs/timestamps with placeholders (`{{programId}}`, `{{eventId}}`, `{{reportId}}`, `{{now}}`).
- Keep JSON keys in stable order as committed.
- For list endpoints, sort returned arrays by `id` in the mock layer when source order is undefined.
