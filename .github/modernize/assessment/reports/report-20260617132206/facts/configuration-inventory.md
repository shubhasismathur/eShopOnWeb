# Configuration & Externalized Settings Inventory

eShopOnWeb uses multiple JSON-based configuration sources across Web, PublicApi, and BlazorAdmin with environment overlays for Development and Docker. Configuration is primarily file-based with environment variable overrides and optional Azure Key Vault integration for non-development execution.

## Configuration Sources

| Source | Type | Path or Location | Notes |
|---|---|---|---|
| Web appsettings | JSON config | `src/Web/appsettings.json` | Base settings including baseUrls and connection strings |
| Web development overlay | JSON config | `src/Web/appsettings.Development.json` | Development logging and local URL overrides |
| Web docker overlay | JSON config | `src/Web/appsettings.Docker.json` | Docker connection strings and container URLs |
| PublicApi appsettings | JSON config | `src/PublicApi/appsettings.json` | Base API URL and connection string defaults |
| PublicApi development overlay | JSON config | `src/PublicApi/appsettings.Development.json` | Development log level and local URL overrides |
| PublicApi docker overlay | JSON config | `src/PublicApi/appsettings.Docker.json` | Docker URL and SQL connection settings |
| BlazorAdmin appsettings | JSON config | `src/BlazorAdmin/wwwroot/appsettings*.json` | Client-side base URL settings per environment |
| Launch profiles Web | launchSettings | `src/Web/Properties/launchSettings.json` | IIS Express and project launch profiles |
| Launch profiles PublicApi | launchSettings | `src/PublicApi/Properties/launchSettings.json` | IIS, project, WSL, and Docker launch profiles |
| Launch profiles BlazorAdmin | launchSettings | `src/BlazorAdmin/Properties/launchSettings.json` | Dev launch profiles for Blazor admin |
| Docker Compose files | Container runtime config | `docker-compose.yml`, `docker-compose.override.yml` | Service env vars, exposed ports, and startup dependencies |
| Azure Key Vault configuration | External secret store reference | `src/Web/Program.cs` | Activated in non-development mode with configured vault endpoint |
| Environment variable source | Runtime configuration provider | Added in `Web/Program.cs` and `PublicApi/Program.cs` | Allows environment overrides for file-based settings |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies or Plugins |
|---|---|---|---|
| Debug | `dotnet build -c Debug` default local build | Developer build and diagnostics | Standard .NET SDK build chain |
| Release | `dotnet build -c Release` and Docker publish | Optimized deployment artifacts | `BuildBundlerMinifier` in Web release build |
| Docker image build | Dockerfile build stages | Containerized release packaging | .NET SDK 8.0 and ASP.NET runtime 8.0 images |
| Central package versioning | Always active via MSBuild property | Consistent dependency versions across projects | `Directory.Packages.props` managed package versions |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` via launchSettings | `appsettings.json` plus `appsettings.Development.json` | Local URLs and development logging verbosity |
| Docker | `ASPNETCORE_ENVIRONMENT=Docker` via compose override | `appsettings.json` plus `appsettings.Docker.json` | Container SQL connections and container URL mappings |
| Production | `ASPNETCORE_ENVIRONMENT=Production` or default runtime | `appsettings.json` and environment variables | Azure Key Vault path in Web and production-grade SQL retrieval |

## Properties Inventory

### Web

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Base, Development, Docker | appsettings files |
| `baseUrls:webBase` | `https://localhost:44315/` | Base, Development, Docker | appsettings files |
| `ConnectionStrings:CatalogConnection` | localdb connection string | Base and Docker override | appsettings files and env override support |
| `ConnectionStrings:IdentityConnection` | localdb connection string | Base and Docker override | appsettings files and env override support |
| `CatalogBaseUrl` | empty | Base | appsettings |
| `Logging:LogLevel:*` | Warning defaults | Development and Docker override | appsettings files |
| `UseOnlyInMemoryDatabase` | false inferred if absent | Any | Configuration provider chain |
| `AZURE_KEY_VAULT_ENDPOINT` | none | Production style paths | environment variable reference |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | none | Production style paths | environment variable reference |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | none | Production style paths | environment variable reference |

### PublicApi

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Base, Development, Docker | appsettings files |
| `baseUrls:webBase` | `https://localhost:5001/` | Base, Development, Docker | appsettings files |
| `ConnectionStrings:CatalogConnection` | localdb connection string | Base and Docker override | appsettings files and env override support |
| `ConnectionStrings:IdentityConnection` | localdb connection string | Base and Docker override | appsettings files and env override support |
| `CatalogBaseUrl` | empty | Base | appsettings |
| `Logging:LogLevel:*` | Warning and Information mix | Development and Docker override | appsettings files |

### BlazorAdmin

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Base, Development, Docker | `wwwroot/appsettings*.json` |
| `baseUrls:webBase` | `https://localhost:44315/` | Base, Development, Docker | `wwwroot/appsettings*.json` |
| `Logging:LogLevel:*` | Information and Warning mix | Development | `wwwroot/appsettings.Development.json` |

## Startup Parameters & Resource Requirements

| Service | JVM or Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Web | ASP.NET Core runtime; URLs set via `ASPNETCORE_URLS`; environment from `ASPNETCORE_ENVIRONMENT` | Not explicitly constrained in repo | Single instance in compose default |
| PublicApi | ASP.NET Core runtime; URLs set via `ASPNETCORE_URLS`; environment from `ASPNETCORE_ENVIRONMENT` | Not explicitly constrained in repo | Single instance in compose default |
| BlazorAdmin | ASP.NET Core hosted runtime under Web app process model | Not explicitly constrained in repo | Served as part of web host |
| SQL Server container | Azure SQL Edge image runtime defaults | Not explicitly constrained in compose | Single container in compose default |

No JVM-specific startup parameters are present because this is a .NET solution.

## Startup Dependency Chain

1. `sqlserver` starts first in Docker Compose and is declared as a dependency for both application services.
2. `eshopwebmvc` waits on compose `depends_on` for `sqlserver` before startup.
3. `eshoppublicapi` waits on compose `depends_on` for `sqlserver` before startup.
4. During app startup, Web and PublicApi both run database seed routines; service readiness depends on database connectivity and completion of seeding logic.
5. Health-check middleware in Web provides runtime readiness signals for UI and API reachability checks.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionStrings:CatalogConnection` | Database connection string | appsettings and Docker overrides `[MASKED]` |
| `ConnectionStrings:IdentityConnection` | Database connection string | appsettings and Docker overrides `[MASKED]` |
| `SA_PASSWORD` | SQL admin password | Docker compose environment `[MASKED]` |
| `AZURE_KEY_VAULT_ENDPOINT` | Secret-store endpoint reference | Environment variable reference |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | Key reference for connection secret | Environment variable reference |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | Key reference for connection secret | Environment variable reference |

### Secrets Provisioning Workflow

Secrets originate from local appsettings for development and Docker environment variables for local container execution. In non-development execution, Web can resolve secrets from Azure Key Vault using chained credentials and environment-specified lookup keys. Access control relies on the hosting identity used by Azure credential providers. Services requiring database connectivity consume catalog and identity connection references during startup and seed execution.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseOnlyInMemoryDatabase` | false when absent | Configuration key and environment override |
| Environment branch for Azure Key Vault path | Disabled in Development and Docker | `ASPNETCORE_ENVIRONMENT` runtime value |

No dedicated feature-flag framework was detected.

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK | 8.0.x | `global.json` |
| .NET Target Framework | net8.0 | `Directory.Packages.props` and csproj files |
| ASP.NET Core shared version | 8.0.2 | `Directory.Packages.props` |
| EF Core packages | 8.0.2 | `Directory.Packages.props` |
| System extensions package set | 8.0.0 | `Directory.Packages.props` |
| Azure Identity package | 1.10.4 | `Directory.Packages.props` |
| Docker build base image (Web) | mcr.microsoft.com/dotnet/sdk:8.0 | `src/Web/Dockerfile` |
| Docker runtime base image (Web) | mcr.microsoft.com/dotnet/aspnet:8.0 | `src/Web/Dockerfile` |
| Docker build base image (PublicApi) | mcr.microsoft.com/dotnet/sdk:8.0 | `src/PublicApi/Dockerfile` |
| Docker runtime base image (PublicApi) | mcr.microsoft.com/dotnet/aspnet:8.0 | `src/PublicApi/Dockerfile` |
