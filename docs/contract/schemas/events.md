# Event schemas

## Resource shape

`Event` response object:
- `id`, `createdDateTime`, `modificationDateTime`
- flattened `EventContent`

`EventContent` request shape (`objectType` tag):
- `objectType`: `EVENT`
- `programID` (required)
- `eventName?`
- `priority`
- `reportDescriptors?[]`
- `intervalPeriod?`
- `intervals[]` (required)
- `payloadDescriptors?[]`
- `targets?`

## Query (`GET /events`)

| Param | Type | Validation |
|---|---|---|
| `programID` | string | optional |
| `skip` | integer | `>= 0`, default `0` |
| `limit` | integer | `1..50`, default `50` |
| `targetType` + `targetValues` | enum + string/array | both-or-neither required |
