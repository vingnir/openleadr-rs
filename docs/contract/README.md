# openleadr-vtn contract docs pack

This folder provides a repo-local, fail-closed HTTP contract map for `openleadr-vtn`.

## Contents
- `endpoints.md`: full route inventory from router wiring.
- `errors.md`: shared error contract (`Problem`) + OAuth error exception.
- `schemas/*.md`: request/response/query schema notes per resource group.
- `examples/*`: deterministic fixture-style request/response pairs.
- `mock-use-cases.md`: fixture composition for producer/VEN flows.

## Role split overview
- **Business/Admin API**: programs/events lifecycle; also current report delete behavior.
- **VEN API**: reports write path (`POST/PUT /reports`), plus read APIs with role-filtered access.
- **VEN management API**: VEN/resource administration.
- **User management API** (internal OAuth feature only): `/users*` and `/auth/token`.

## Reproducibility/capture notes
- Intended runtime capture flow (when Docker available):
  1. `docker compose up -d db`
  2. `cargo sqlx migrate run`
  3. `RUST_LOG=trace cargo run --bin openleadr-vtn`
  4. execute scripted curl sequence and redact dynamic fields.
- In this commit, runtime capture could not be executed because Docker was unavailable in the execution environment (`docker: command not found`). Example files are deterministic contract fixtures derived from router + wire types.

## Updating the contract pack
1. Re-run endpoint inventory against `state.rs` router.
2. Re-check extractor/query structs in `api/*.rs` for validation and role gates.
3. Refresh examples and keep placeholders stable.
4. Keep endpoint table sorted by path then method.

## Placeholder conventions
- `{{jwt}}`, `{{businessJwt}}`, `{{venJwt}}` for tokens.
- `{{programId}}`, `{{eventId}}`, `{{reportId}}`, `{{venId}}` for dynamic IDs.
- `{{now}}` for generated timestamps.
- `{{uuid}}` for `Problem.instance`.
