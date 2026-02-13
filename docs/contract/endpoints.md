# openleadr-vtn endpoint inventory (router truth)

Source of truth: `openleadr-vtn/src/state.rs::router_without_state()`.
Sorted by path (lexicographic), then method order GET/POST/PUT/PATCH/DELETE.

| Path | Method | Handler | Module | Auth gate (extractor/runtime) | Request extractor(s) | Success response |
|---|---|---|---|---|---|---|
| `/auth/token` | POST | `auth::token` | `api/auth.rs` | none (OAuth client-credentials validation inside handler). Route exists only with `internal-oauth` feature. | `ValidatedForm<AccessTokenRequest>` + headers (`Authorization: Basic ...` or form client credentials) | `200` JSON `AccessTokenResponse` |
| `/events` | GET | `event::get_all` | `api/event.rs` | `User` (any valid bearer token) | `ValidatedQuery<event::QueryParams>` | `200` JSON `Vec<Event>` |
| `/events` | POST | `event::add` | `api/event.rs` | `BusinessUser` (must be business role) | `ValidatedJson<EventContent>` | `201` JSON `Event` |
| `/events/{id}` | GET | `event::get` | `api/event.rs` | `User` | `Path<EventId>` | `200` JSON `Event` |
| `/events/{id}` | PUT | `event::edit` | `api/event.rs` | `BusinessUser` | `Path<EventId>`, `ValidatedJson<EventContent>` | `200` JSON `Event` |
| `/events/{id}` | DELETE | `event::delete` | `api/event.rs` | `BusinessUser` | `Path<EventId>` | `200` JSON `Event` |
| `/health` | GET | `healthcheck` | `api/mod.rs` | none | none | `200` text `OK` |
| `/programs` | GET | `program::get_all` | `api/program.rs` | `User` | `ValidatedQuery<program::QueryParams>` | `200` JSON `Vec<Program>` |
| `/programs` | POST | `program::add` | `api/program.rs` | `BusinessUser` | `ValidatedJson<ProgramContent>` | `201` JSON `Program` |
| `/programs/{id}` | GET | `program::get` | `api/program.rs` | `User` | `Path<ProgramId>` | `200` JSON `Program` |
| `/programs/{id}` | PUT | `program::edit` | `api/program.rs` | `BusinessUser` | `Path<ProgramId>`, `ValidatedJson<ProgramContent>` | `200` JSON `Program` |
| `/programs/{id}` | DELETE | `program::delete` | `api/program.rs` | `BusinessUser` | `Path<ProgramId>` | `200` JSON `Program` |
| `/reports` | GET | `report::get_all` | `api/report.rs` | `User` | `ValidatedQuery<report::QueryParams>` | `200` JSON `Vec<Report>` |
| `/reports` | POST | `report::add` | `api/report.rs` | `VENUser` (must include at least one VEN role) | `ValidatedJson<ReportContent>` | `201` JSON `Report` |
| `/reports/{id}` | GET | `report::get` | `api/report.rs` | `User` | `Path<ReportId>` | `200` JSON `Report` |
| `/reports/{id}` | PUT | `report::edit` | `api/report.rs` | `VENUser` | `Path<ReportId>`, `ValidatedJson<ReportContent>` | `200` JSON `Report` |
| `/reports/{id}` | DELETE | `report::delete` | `api/report.rs` | `BusinessUser` (code comment notes this differs from spec intent) | `Path<ReportId>` | `200` JSON `Report` |
| `/users` | GET | `user::get_all` | `api/user.rs` | `UserManagerUser` | none | `200` JSON `Vec<UserDetails>` |
| `/users` | POST | `user::add_user` | `api/user.rs` | `UserManagerUser` | `ValidatedJson<NewUser>` | `201` JSON `UserDetails` |
| `/users/{id}` | GET | `user::get` | `api/user.rs` | `UserManagerUser` | `Path<String>` | `200` JSON `UserDetails` |
| `/users/{id}` | POST | `user::add_credential` | `api/user.rs` | `UserManagerUser` | `Path<String>`, `ValidatedJson<NewCredential>` | `200` JSON `CreatedCredential` |
| `/users/{id}` | PUT | `user::edit` | `api/user.rs` | `UserManagerUser` | `Path<String>`, `ValidatedJson<NewUser>` | `200` JSON `UserDetails` |
| `/users/{id}` | DELETE | `user::delete_user` | `api/user.rs` | `UserManagerUser` | `Path<String>` | `200` JSON `UserDetails` |
| `/users/{user_id}/{client_id}` | DELETE | `user::delete_credential` | `api/user.rs` | `UserManagerUser` | `Path<(String, String)>` | `200` JSON `DeletedCredential` |
| `/vens` | GET | `ven::get_all` | `api/ven.rs` | `User` (internally narrowed to VenManager or own VEN via data-source filtering) | `ValidatedQuery<ven::QueryParams>` | `200` JSON `Vec<Ven>` |
| `/vens` | POST | `ven::add` | `api/ven.rs` | `VenManagerUser` | `ValidatedJson<VenContent>` | `201` JSON `Ven` |
| `/vens/{id}` | GET | `ven::get` | `api/ven.rs` | `User` | `Path<VenId>` | `200` JSON `Ven` |
| `/vens/{id}` | PUT | `ven::edit` | `api/ven.rs` | `VenManagerUser` | `Path<VenId>`, `ValidatedJson<VenContent>` | `200` JSON `Ven` |
| `/vens/{id}` | DELETE | `ven::delete` | `api/ven.rs` | `VenManagerUser` | `Path<VenId>` | `200` JSON `Ven` |
| `/vens/{ven_id}/resources` | GET | `resource::get_all` | `api/resource.rs` | `User` + runtime gate `has_write_permission` (VenManager or matching VEN id) | `Path<VenId>`, `ValidatedQuery<resource::QueryParams>` | `200` JSON `Vec<Resource>` |
| `/vens/{ven_id}/resources` | POST | `resource::add` | `api/resource.rs` | `User` + runtime gate `has_write_permission` | `Path<VenId>`, `ValidatedJson<ResourceContent>` | `201` JSON `Resource` |
| `/vens/{ven_id}/resources/{id}` | GET | `resource::get` | `api/resource.rs` | `User` + runtime gate `has_write_permission` | `Path<(VenId, ResourceId)>` | `200` JSON `Resource` |
| `/vens/{ven_id}/resources/{id}` | PUT | `resource::edit` | `api/resource.rs` | `User` + runtime gate `has_write_permission` | `Path<(VenId, ResourceId)>`, `ValidatedJson<ResourceContent>` | `200` JSON `Resource` |
| `/vens/{ven_id}/resources/{id}` | DELETE | `resource::delete` | `api/resource.rs` | `User` + runtime gate `has_write_permission` | `Path<(VenId, ResourceId)>` | `200` JSON `Resource` |

## Fail-closed coverage note

- Unknown paths hit router `fallback(handler_404)` and return a `Problem` with `404`.
- Unknown methods for known paths hit middleware `method_not_allowed` and return `Problem` with `405`.
