# PetroffCenter — Technical documentation

This folder documents the `PetroffSoft.PetroffCenter` solution (API + Blazor WebAssembly UI + supporting services) based on what is implemented in this repository.

## Solution overview

| Component | Path | What it does |
|---|---|---|
| API | `PetroffSoft.PetroffCenter.Api/` | ASP.NET Core API (`/api/v1/*`), Swagger (Development/Test), health checks. |
| Web UI | `PetroffSoft.PetroffCenter.WebApp/` | Blazor WebAssembly UI (MudBlazor + Fluxor) consuming the API. |
| Invoice sender | `PetroffSoft.PetroffCenter.InvoiceSenderAF/` | Azure Functions worker for invoice-related background tasks (SendGrid + reporting). |
| Reports | `PetroffCenter.Reports/` | Reporting-related code used by the UI/services. |
| Shared | `PetroffSoftPetroffCenter.Shared/` | Shared validators/models used across projects. |
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
