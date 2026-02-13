# Program schemas

## Resource shape

`Program` response object = envelope + `ProgramContent`.

Core response fields:
- `id` (string)
- `createdDateTime` (RFC3339)
- `modificationDateTime` (RFC3339)
- all `ProgramContent` fields below

`ProgramContent` request fields (`objectType` tagged):
- `objectType`: must be `PROGRAM`
- `programName`: string, length 1..128 (required)
- `programLongName?`, `retailerName?`, `retailerLongName?`, `programType?`, `country?`, `principalSubdivision?`
- `timeZoneOffset?` (ISO8601 duration)
- `intervalPeriod?`
- `programDescriptions?[]`
- `bindingEvents?`, `localPrice?`
- `payloadDescriptors?[]`
- `targets?`

## Query (`GET /programs`)

| Param | Type | Validation |
|---|---|---|
| `skip` | integer | `>= 0`, default `0` |
| `limit` | integer | `1..50`, default `50` |
| `targetType` + `targetValues` | enum + string/array | must be set together or omitted together |

`targetValues` accepts either single string or repeated/array values.
