# Configuration & Externalized Settings Inventory

Configuration is layered through standard ASP.NET Core settings files, environment-specific overrides, environment variables, and an optional Azure Key Vault production path. The repository also carries Docker, launch-profile, and user-secrets settings that shape local development and containerized execution.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| `appsettings.json` | JSON config | `src/Web`, `src/PublicApi`, `src/BlazorAdmin/wwwroot` | Base application settings, base URLs, connection strings, logging |
| `appsettings.Development.json` | JSON config | `src/Web`, `src/PublicApi`, `src/BlazorAdmin/wwwroot` | Development overrides and localhost URLs |
| `appsettings.Docker.json` | JSON config | `src/Web`, `src/PublicApi`, `src/BlazorAdmin/wwwroot` | Containerized URLs and SQL Server connection strings |
| `launchSettings.json` | Launch profile config | `src/Web/Properties`, `src/PublicApi/Properties`, `src/BlazorAdmin/Properties` | Local profile URLs and `ASPNETCORE_ENVIRONMENT` values |
| `docker-compose.yml` and override | Compose config | repository root | Service topology, ports, environment variables, SQL Server container |
| Environment variables | Runtime overrides | `Program.cs` in `Web` and `PublicApi` | Loaded by `AddEnvironmentVariables()` |
| User secrets | Local secret storage | User-secrets IDs in `.csproj` | Local developer secret injection |
| Azure Key Vault | External secret store | URI from `AZURE_KEY_VAULT_ENDPOINT` | Used only by the production `Web` branch |
| `global.json` | SDK config | repository root | Pins .NET SDK major line with `latestFeature` roll forward |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| `Debug` | Standard .NET build configuration | Local development and diagnostics | Default SDK behavior |
| `Release` | Standard .NET build configuration | Optimized builds and publish output | `BuildBundlerMinifier` is marked private for build-time use |
| Docker image build | `docker-compose` or Docker launch profile | Build and run `Web` and `PublicApi` in containers | Multi-stage .NET 8 Dockerfiles |
| Central package management | Automatic via MSBuild | Keep package versions aligned across projects | `Directory.Packages.props` |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` | `appsettings.json` + `appsettings.Development.json` | Localhost base URLs, developer diagnostics |
| Production | `ASPNETCORE_ENVIRONMENT=Production` or default host deployment | `appsettings.json` + environment variables | Azure Key Vault path, HSTS, exception handler |
| Docker | `ASPNETCORE_ENVIRONMENT=Docker` from compose override | `appsettings.json` + `appsettings.Docker.json` | Container URLs, SQL Server hostname, HTTP port 8080 |
| Test support | `appsettings.test.json` for `PublicApi` test host or `UseOnlyInMemoryDatabase=true` | Test project settings and environment values | Enables deterministic test configuration and in-memory persistence |

## Properties Inventory

### Web

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Development, Docker override | `appsettings*.json` |
| `baseUrls:webBase` | `https://localhost:44315/` or localhost 5001 in launch profile | Development, Docker override | `appsettings*.json`, launch settings |
| `ConnectionStrings:CatalogConnection` | LocalDB connection | Docker and production override | `appsettings*.json`, Azure Key Vault indirection |
| `ConnectionStrings:IdentityConnection` | LocalDB connection | Docker and production override | `appsettings*.json`, Azure Key Vault indirection |
| `CatalogBaseUrl` | empty string | Optional override | `appsettings.json`, environment variables |
| `AZURE_KEY_VAULT_ENDPOINT` | unset | Production | Environment variable |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | unset | Production | Environment variable |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | unset | Production | Environment variable |
| `UseOnlyInMemoryDatabase` | `false` when absent | Optional test-style override | Environment variable or custom config |

### PublicApi

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Docker override | `appsettings*.json` |
| `baseUrls:webBase` | `https://localhost:5001/` | Docker override | `appsettings*.json` |
| `ConnectionStrings:CatalogConnection` | LocalDB connection | Docker override | `appsettings*.json` |
| `ConnectionStrings:IdentityConnection` | LocalDB connection | Docker override | `appsettings*.json` |
| `CatalogBaseUrl` | empty string | Optional override | `appsettings.json` |
| `ASPNETCORE_URLS` | launch profile specific | Docker override | launch settings, compose override |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| `Web` | No explicit runtime flags in repo; standard ASP.NET Core host | Not specified | Not specified |
| `PublicApi` | No explicit runtime flags in repo; standard ASP.NET Core host | Not specified | Not specified |
| `SQL Server` container | `ACCEPT_EULA=Y` and password environment variable in compose | Not specified | 1 in compose |

## Startup Dependency Chain

1. `sqlserver` starts first in Docker Compose and is declared as a dependency of both application containers.
2. `Web` and `PublicApi` build their service collections, register DbContexts, and seed their databases during startup.
3. In production, `Web` resolves Azure Key Vault configuration before creating SQL Server DbContexts.
4. Health-check endpoints become available after host startup completes; there is no explicit retry-orchestration layer in Compose beyond service dependency ordering.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionStrings:CatalogConnection` | Database connection string | `appsettings*.json` or Azure Key Vault indirection, secret value masked |
| `ConnectionStrings:IdentityConnection` | Database connection string | `appsettings*.json` or Azure Key Vault indirection, secret value masked |
| `AZURE_KEY_VAULT_ENDPOINT` | Secret-store endpoint | Environment variable |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | Key Vault lookup key | Environment variable |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | Key Vault lookup key | Environment variable |
| User-secrets IDs in project files | Local development secret mapping | Project metadata only |
| JWT secret constant | Application auth secret | Source code constant, value intentionally omitted here |

### Secrets Provisioning Workflow

Local development relies on appsettings files, launch profiles, and optional user-secrets stores. Docker execution injects environment variables and compose-managed SQL Server settings. The production web-host branch uses a chained Azure credential, resolves the Key Vault endpoint from environment configuration, then looks up the named SQL connection strings before constructing retry-enabled SQL Server DbContexts.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseOnlyInMemoryDatabase` | Off when absent | Custom configuration value or environment variable |
| `ASPNETCORE_ENVIRONMENT` | Host default | Launch settings, Docker Compose, deployment environment |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---:|---|
| .NET target framework | net8.0 | `Directory.Packages.props` |
| ASP.NET Core packages | 8.0.2 | `Directory.Packages.props` |
| Entity Framework Core | 8.0.2 | `Directory.Packages.props` |
| Azure Identity | 1.10.4 | `Directory.Packages.props` |
| Swashbuckle.AspNetCore | 6.5.0 | `Directory.Packages.props` |
| MediatR | 12.0.1 | `Directory.Packages.props` |
| Docker base image `Web` | `mcr.microsoft.com/dotnet/aspnet:8.0` and SDK `8.0` | `src/Web/Dockerfile` |
| Docker base image `PublicApi` | `mcr.microsoft.com/dotnet/aspnet:8.0` and SDK `8.0` | `src/PublicApi/Dockerfile` |
