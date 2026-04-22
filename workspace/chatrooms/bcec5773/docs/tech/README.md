# PetroffCenter — Technical documentation

This folder contains technical documentation related to PetroffCenter.

> **Note:** The runnable `.NET` application projects referenced by this documentation (the API, Web UI, Azure Functions worker, shared libraries, and E2E tests) are **not included in this repository**. This repo should be treated as documentation/artifact storage only. The solution files, `.csproj` files, and source code are maintained in a separate repository.

## Scope of this folder

The files in `docs/tech/` describe the PetroffCenter architecture and API at a documentation level. They do **not** provide a buildable local source layout for:

- `PetroffSoft.PetroffCenter.Api`
- `PetroffSoft.PetroffCenter.WebApp`
- `PetroffSoft.PetroffCenter.InvoiceSenderAF`
- `PetroffCenter.Reports`
- `PetroffSoft.PetroffCenter.Shared`
- `PetroffSoft.PetroffCenter.E2ETests`

## Solution overview

| Component | Path | What it does |
|---|---|---|
| API | `PetroffSoft.PetroffCenter.Api/` | ASP.NET Core API (`/api/v1/*`), Swagger (Development/Test), health checks. |
| Web UI | `PetroffSoft.PetroffCenter.WebApp/` | Blazor WebAssembly UI (MudBlazor + Fluxor) consuming the API. |
| Invoice sender | `PetroffSoft.PetroffCenter.InvoiceSenderAF/` | Azure Functions worker for invoice-related background tasks (SendGrid + reporting). |
| Reports | `PetroffCenter.Reports/` | Reporting-related code used by the UI/services. |
| Shared | `PetroffSoft.PetroffCenter.Shared/` | Shared validators/models used across projects. |
| E2E tests | `PetroffSoft.PetroffCenter.E2ETests/` | End-to-end test project. |

## Prerequisites

- `.NET SDK` `8.x` (all main projects target `net8.0`)
- SQL Server reachable via `ConnectionStrings:DefaultConnection` (used by `TokenServerDbContext`)
- Access to PetroffSoft private NuGet feeds (if your environment does not already have them configured)

## Local run (developer workflow)

1. Restore and build:
   - `dotnet restore`
   - `dotnet build`
2. Configure secrets/config:
   - API needs token signing keys (see below).
   - Set a valid `ConnectionStrings:DefaultConnection`.
3. Run the API:
   - `dotnet run --project PetroffSoft.PetroffCenter.Api`
4. Run the Web UI:
   - `dotnet run --project PetroffSoft.PetroffCenter.WebApp`

The exact URLs/ports depend on your local profile; use the console output from `dotnet run`.

## API configuration essentials

The API uses two signing keys:

- `PublicTokenKeyBase64Encoded` (read from configuration, e.g. `appsettings.*.json`)
- `PrivateTokenKeyBase64Encoded` (typically stored in user-secrets for development)

See `docs/tech/architecture.md` for how the token is used at runtime, and `docs/tech/api.md` for the API surface.
