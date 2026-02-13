# Resource schemas

## Resource shape

`Resource` response object:
- `id`, `createdDateTime`, `modificationDateTime`
- flattened `ResourceContent`

`ResourceContent` request shape:
- `objectType`: `RESOURCE`
- `resourceName` (required)
- `venID` (server-associated via path; may appear depending on serialization)
- `attributes?[]`
- `targets?`

## Query (`GET /vens/{ven_id}/resources`)

| Param | Type | Validation |
|---|---|---|
| `resourceName` | string | length `1..128` |
| `skip` | integer | `>= 0`, default `0` |
| `limit` | integer | `1..50`, default `50` |
| `targetType` + `targetValues` | enum + string/array | both-or-neither required |

## Authorization rule

All resource endpoints require either:
- `VenManager` role, or
- `VEN` role containing the same `{ven_id}` as the route.
