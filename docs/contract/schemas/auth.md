# Auth schemas

## `POST /auth/token` (internal OAuth only)

**Content-Type:** `application/x-www-form-urlencoded` (validated by `ValidatedForm`).

### Request form

| Field | Type | Required | Notes |
|---|---|---|---|
| `grant_type` | string | yes | Must be exactly `client_credentials`. |
| `client_id` | string | conditional | Required unless supplied via Basic auth header. |
| `client_secret` | string | conditional | Required unless supplied via Basic auth header. |

Rules:
- Must provide credentials either in Basic auth header OR in form fields, not both.
- Bearer token in `Authorization` is ignored for this endpoint.

### Success response (`200`)

```json
{
  "access_token": "{{jwt}}",
  "token_type": "Bearer",
  "expires_in": 2592000,
  "scope": null
}
```

### Error response
- OAuth error object (not `Problem`) for invalid client/grant/etc.
- `error` values from `openleadr_wire::oauth::OAuthErrorType`.
