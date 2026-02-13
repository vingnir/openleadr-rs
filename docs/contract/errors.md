# Error contract

## Standard API error payload (`Problem`)

Most VTN endpoints return `application/json` body:

```json
{
  "type": "about:blank",
  "title": "400 Bad Request",
  "status": 400,
  "detail": "...",
  "instance": "{{uuid}}"
}
```

`instance` is generated dynamically; use placeholders in fixtures.

## Status mapping (from `AppError`)

- `400`: validation, malformed JSON/form/query, identifier parse errors, bad request.
- `401`: missing bearer auth (`Auth(...)`).
- `403`: forbidden role or access scope violations.
- `404`: unknown path/object not found.
- `405`: method not allowed on known path.
- `409`: DB conflict + foreign key errors (mapped as conflict).
- `415`: unsupported media type.
- `500`: storage/SQL/internal failures.

## OAuth endpoint exception

`POST /auth/token` returns OAuth RFC-style payload instead of `Problem`:

```json
{
  "error": "invalid_client",
  "error_description": "..."
}
```

with status depending on error (`400`, `401`, `500`, or `404` when OAuth disabled).
