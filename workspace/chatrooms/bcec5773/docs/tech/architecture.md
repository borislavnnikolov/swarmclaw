# Architecture

> **Note:** This repository snapshot contains documentation only. The implementation projects, solution files, and source paths referenced below are **not present in this repo**, so this page should be read as an external/inferred architecture summary rather than a repo-verified description of the current workspace contents.

PetroffCenter is a multi-project `.NET 8` solution with an ASP.NET Core API and a Blazor WebAssembly front-end. The UI calls the API (`/api/v1/*`), and the API persists to SQL Server via `TokenServerDbContext`.

## High-level topology

1. `PetroffSoft.PetroffCenter.WebApp` (Blazor WebAssembly)
2. `PetroffSoft.PetroffCenter.Api` (ASP.NET Core API)
3. SQL Server (used by `TokenServerDbContext`)

## Components

| Component | Path | Notes |
|---|---|---|
| Web UI | `PetroffSoft.PetroffCenter.WebApp/` | Blazor WebAssembly app; uses MudBlazor for UI and Fluxor for state. |
| API | `PetroffSoft.PetroffCenter.Api/` | Controllers under `PetroffSoft.PetroffCenter.Api/Controllers/`; Swagger is enabled in `Development` and `Test`. |
| Invoice sender | `PetroffSoft.PetroffCenter.InvoiceSenderAF/` | Azure Functions worker; shares service layer and DB access patterns with the API. |
| Data access | `PetroffSoft.TokenServer.*` projects | Domain/data layer for `TokenServerDbContext` + services used by both API and functions. |
| Shared | `PetroffSoft.PetroffCenter.Shared/` | Shared validators (e.g. `PasswordValidator`). |

## Authentication & authorization

- The API uses JWT Bearer authentication (Swagger defines a `Bearer` security scheme).
- Most controllers inherit `BaseTokenController`, which applies:
  - base route: `api/v1/[controller]`
  - default authorization: `PetroffTokenApiAuthorize(TOKEN_PERMISSIONS.MAIN_ACCESS)`
- `TokenController` (`/api/v1/Token/*`) issues and validates tokens and exposes the public signing key (`GET /api/v1/Token/key`).

See `docs/tech/api.md` for endpoint-level notes.

## Localization

- API: `UseCultureMiddleware` sets `DefaultCulture = "en"` and supports `"en"` and `"bg-BG"`.
- Web UI: `AddLocalizationDynamic(...)` defaults to `"bg-BG"` and supports `"en"` and `"bg-BG"` with satellite assemblies.

## Health & observability

- Health endpoints:
  - `/health` (default health checks)
  - `/health/ready` (readiness; includes DB check)
  - `/health/live` (liveness; always healthy)
- Logging/telemetry:
  - Serilog configured via `IConfiguration` + console/debug sinks
  - Application Insights telemetry is enabled in both API and Azure Functions worker
