# VEN schemas

## Resource shape

`Ven` response object:
- `id`, `createdDateTime`, `modificationDateTime`
- flattened `VenContent`

`VenContent` request shape:
- `objectType`: `VEN`
- `venName` (required)
- `attributes?[]`
- `targets?`
- `resources?[]` (optional embedded resource list)

## Query (`GET /vens`)

| Param | Type | Validation |
|---|---|---|
| `venName` | string | length `1..128` |
| `skip` | integer | `>= 0`, default `0` |
| `limit` | integer | `1..50`, default `50` |
| `targetType` + `targetValues` | enum + string/array | both-or-neither required |
