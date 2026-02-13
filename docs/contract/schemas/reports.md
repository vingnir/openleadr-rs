# Report schemas

## Resource shape

`Report` response object:
- `id`, `createdDateTime`, `modificationDateTime`
- flattened `ReportContent`

`ReportContent` request shape:
- `objectType`: `REPORT`
- `programID` (required)
- `eventID` (required)
- `clientName` (required)
- `reportName?` (if present length 1..128)
- `payloadDescriptors?[]` (private names also constrained to length 1..128)
- `resources[]`

## Query (`GET /reports`)

| Param | Type | Validation |
|---|---|---|
| `programID` | string | optional |
| `eventID` | string | optional |
| `clientName` | string | length `1..128` |
| `skip` | integer | default `0` |
| `limit` | integer | `<= 50`, default `50` |

## Authorization split

- `POST /reports`, `PUT /reports/{id}`: `VENUser` only.
- `DELETE /reports/{id}`: `BusinessUser` in current implementation.
