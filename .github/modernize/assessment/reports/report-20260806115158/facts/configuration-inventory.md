# Configuration & Externalized Settings Inventory

eShopOnWeb uses a layered ASP.NET Core configuration model with JSON file overrides per environment (Development, Docker, Production), environment variables, and Azure Key Vault for production secrets — totalling 4 configuration file sets across 3 services plus a Blazor WASM client.

## Configuration Sources

| Source | Type | Path / Location | Notes |
|---|---|---|---|
| `appsettings.json` | JSON file | `src/Web/`, `src/PublicApi/`, `src/BlazorAdmin/wwwroot/` | Base defaults for all environments |
| `appsettings.Development.json` | JSON file | `src/Web/`, `src/PublicApi/`, `src/BlazorAdmin/wwwroot/` | Development overrides (verbose logging, local URLs) |
| `appsettings.Docker.json` | JSON file | `src/Web/`, `src/PublicApi/`, `src/BlazorAdmin/wwwroot/` | Docker Compose overrides (SQL Server container connection strings, internal Docker URLs) |
| `launchSettings.json` | JSON file | `src/Web/Properties/`, `src/PublicApi/Properties/`, `src/BlazorAdmin/Properties/` | Local developer launch profiles only; NOT deployed |
| Environment variables | OS / Docker environment | `docker-compose.yml` `environment:` sections | SA_PASSWORD, ACCEPT_EULA for SQL Server container; ASPNETCORE_ENVIRONMENT for app services |
| Azure Key Vault | Secret store | URI from `AZURE_KEY_VAULT_ENDPOINT` env var | Production only; loaded via `Azure.Extensions.AspNetCore.Configuration.Secrets` with `ChainedTokenCredential` (AzureDeveloperCliCredential → DefaultAzureCredential) |
| User Secrets | .NET User Secrets | `UserSecretsId` in `Web.csproj` and `PublicApi.csproj` | Development only; local developer override for connection strings and secrets |
| `global.json` | SDK pinning | `/global.json` (repo root) | Pins .NET SDK to 8.0.x with `latestFeature` roll-forward |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies / Plugins |
|---|---|---|---|
| Debug | Default in Visual Studio / `dotnet build` | Development builds; includes debug symbols, no optimization | None additional |
| Release | `-c Release` / CI publish | Production-optimized build; enables `BuildBundlerMinifier` for CSS/JS minification | `BuildBundlerMinifier` (conditional on Release config in `Web.csproj`) |

No Maven/Gradle multi-profile build system; the .NET SDK uses MSBuild `Configuration` property only.

## Runtime Profiles

| Profile | Activation Method | Config Files Applied | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` (default in launchSettings) | `appsettings.json` + `appsettings.Development.json` | Verbose logging (Debug/Information), local SQL Server (LocalDB), Azure CLI credential, Swagger UI enabled |
| Docker | `ASPNETCORE_ENVIRONMENT=Docker` | `appsettings.json` + `appsettings.Docker.json` | SQL Server container connection strings (`Server=sqlserver,1433`), Docker-internal URLs |
| Production | `ASPNETCORE_ENVIRONMENT=Production` | `appsettings.json` (no production-specific JSON file) | Connection strings read from Azure Key Vault; HSTS and HTTPS redirection enforced; Swagger UI disabled |

Profile selection is controlled entirely by the `ASPNETCORE_ENVIRONMENT` environment variable. There are no additional `@Profile` annotations or Spring-style combined profiles; the `launchSettings.json` for `Web` includes a `Web - PROD` profile that sets `ASPNETCORE_ENVIRONMENT=Production` for local production-mode testing.

## Properties Inventory

### Web Service (`src/Web/appsettings*.json`)

| Property Key | Default Value | Profile Override | Source |
|---|---|---|---|
| `ConnectionStrings:CatalogConnection` | `Server=(localdb)\mssqllocaldb;...CatalogDb` | Docker: `Server=sqlserver,1433;...` / Production: Azure Key Vault | JSON file / Key Vault |
| `ConnectionStrings:IdentityConnection` | `Server=(localdb)\mssqllocaldb;...Identity` | Docker: `Server=sqlserver,1433;...` / Production: Azure Key Vault | JSON file / Key Vault |
| `AZURE_KEY_VAULT_ENDPOINT` | *(empty / unset)* | Production: set via env var | Environment variable |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | *(empty / unset)* | Production: name of Key Vault secret | Environment variable |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | *(empty / unset)* | Production: name of Key Vault secret | Environment variable |
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Docker: `http://localhost:5200/api/` | JSON file |
| `baseUrls:webBase` | `https://localhost:44315/` | Docker: `http://host.docker.internal:5106/` | JSON file |
| `CatalogBaseUrl` | `""` | — | JSON file |
| `Logging:LogLevel:Default` | `Warning` | Development/Docker: `Debug` | JSON file |

### PublicApi Service (`src/PublicApi/appsettings*.json`)

| Property Key | Default Value | Profile Override | Source |
|---|---|---|---|
| `ConnectionStrings:CatalogConnection` | `Server=(localdb)\mssqllocaldb;...CatalogDb` | Docker: `Server=sqlserver,1433;...` | JSON file |
| `ConnectionStrings:IdentityConnection` | `Server=(localdb)\mssqllocaldb;...Identity` | Docker: `Server=sqlserver,1433;...` | JSON file |
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Docker: `http://localhost:5200/api/` | JSON file |
| `baseUrls:webBase` | `https://localhost:5001/` | Docker: `http://host.docker.internal:5106/` | JSON file |
| `CatalogBaseUrl` | `""` | — | JSON file |
| `Logging:LogLevel:Default` | `Warning` | Development: `Information` | JSON file |

### BlazorAdmin Client (`src/BlazorAdmin/wwwroot/appsettings*.json`)

| Property Key | Default Value | Profile Override | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Docker: `http://localhost:5200/api/` | JSON file (served as static asset) |
| `baseUrls:webBase` | `https://localhost:44315/` | Docker: `http://host.docker.internal:5106/` | JSON file |
| `Logging:LogLevel:Default` | `Information` | — | JSON file |

## Startup Parameters & Resource Requirements

| Service | Runtime Options | Memory / CPU | Instance Count | Notes |
|---|---|---|---|---|
| Web (eshopwebmvc) | `ASPNETCORE_ENVIRONMENT` env var | Not configured in Docker Compose (no `mem_limit`) | 1 | Depends on sqlserver container |
| PublicApi (eshoppublicapi) | `ASPNETCORE_ENVIRONMENT` env var | Not configured in Docker Compose | 1 | Depends on sqlserver container |
| sqlserver | `SA_PASSWORD`, `ACCEPT_EULA` | Not configured in Docker Compose | 1 | Azure SQL Edge image (`mcr.microsoft.com/azure-sql-edge`) |

No JVM heap settings (not applicable for .NET). No Kubernetes manifests or resource limits were found in the repository. No horizontal scaling configuration was detected.

## Startup Dependency Chain

```
sqlserver (port 1433)
    ↑ depends_on (Docker Compose)
    ├── eshopwebmvc
    └── eshoppublicapi
```

Both application services declare `depends_on: sqlserver` in `docker-compose.yml`, which ensures container creation order but does NOT wait for SQL Server readiness (no `condition: service_healthy`). As a result, both services may attempt to connect before SQL Server is accepting connections; EF Core's `EnableRetryOnFailure()` (production path) mitigates this by retrying transient SQL failures. No `dockerize` or health-check-based wait mechanism is in place.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage |
|---|---|---|
| `ConnectionStrings:CatalogConnection` | SQL Server connection string (with credentials in Docker profile) | appsettings.Docker.json (SA password in plain text); Production: Azure Key Vault |
| `ConnectionStrings:IdentityConnection` | SQL Server connection string (with credentials in Docker profile) | appsettings.Docker.json (SA password in plain text); Production: Azure Key Vault |
| `SA_PASSWORD` | SQL Server SA account password | docker-compose.yml environment section (plain text `@someThingComplicated1234`) |
| `AZURE_KEY_VAULT_ENDPOINT` | Azure Key Vault URI | Environment variable (production) |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | Key Vault secret name for catalog DB | Environment variable (production) |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | Key Vault secret name for identity DB | Environment variable (production) |
| JWT signing key | HMAC key for JWT token signing | Not visible in config files; expected to be provisioned via Key Vault or user secrets |
| `UserSecretsId` (Web) | `aspnet-Web2-1FA3F72E-E7E3-4360-9E49-1CCCD7FE85F7` | .NET User Secrets store (development only) |
| `UserSecretsId` (PublicApi) | `5b662463-1efd-4bae-bde4-befe0be3e8ff` | .NET User Secrets store (development only) |

> **Warning**: `SA_PASSWORD=@someThingComplicated1234` is committed in plain text in `docker-compose.yml` and `appsettings.Docker.json`. This is acceptable for local development but should not be used in shared or production environments.

### Secrets Provisioning Workflow

**Development**: Developers use .NET User Secrets (`dotnet user-secrets`) to override connection strings locally. The `UserSecretsId` in each `.csproj` identifies the secrets store. No additional setup is required for the in-memory development profile.

**Docker Compose**: Connection strings with the SQL Server SA password are embedded directly in `appsettings.Docker.json`. This is convenient but insecure; the SA password is also set as a plain-text environment variable in `docker-compose.yml`.

**Production (Azure)**: 
1. The `AZURE_KEY_VAULT_ENDPOINT` environment variable is set on the Azure App Service / Container App during deployment.
2. `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` and `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` point to the names of secrets in Key Vault.
3. At startup, the app authenticates to Key Vault using `ChainedTokenCredential` (tries `AzureDeveloperCliCredential` first for developer CLI access, then falls back to `DefaultAzureCredential` for managed identity in production).
4. The Key Vault provider loads the connection string secrets into the ASP.NET Core configuration pipeline; the app consumes them via standard `builder.Configuration[key]` calls.

Required Azure RBAC: The hosting identity (managed identity or service principal) needs `Key Vault Secrets User` role on the Key Vault resource.

## Feature Flags

No feature flag framework (LaunchDarkly, .NET `Microsoft.FeatureManagement`, Unleash, etc.) was detected. No `@ConditionalOnProperty` equivalent or conditional service registrations based on runtime flags were found. The only conditional behavior is environment-based (Development vs Production code paths in `Program.cs`), which is standard profile-based configuration rather than a feature flag system.

| Flag Name | Default | Controlled By | Notes |
|---|---|---|---|
| Swagger UI | Enabled in Development | `ASPNETCORE_ENVIRONMENT` | Swagger is registered unconditionally in `PublicApi/Program.cs` and served at `/swagger` in all environments — no `IsDevelopment()` guard on Swagger middleware |
| In-Memory DB | Enabled in Development/Docker | `ASPNETCORE_ENVIRONMENT` | `IsDevelopment() or EnvironmentName == "Docker"` branch in `Program.cs` determines SQL Server vs in-memory |
| Azure Key Vault | Disabled in Development | `ASPNETCORE_ENVIRONMENT` | Only loaded in non-Development, non-Docker environments |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK | 8.0.x (latestFeature roll-forward) | `global.json` |
| ASP.NET Core | 8.0.2 | `Directory.Packages.props` (`AspNetVersion`) |
| EF Core | 8.0.2 | `Directory.Packages.props` (`EntityFramworkCoreVersion`) |
| Blazor WebAssembly | 8.0.2 | `Directory.Packages.props` |
| Target Framework | `net8.0` | `Directory.Packages.props` (`TargetFramework`) |
| Docker base image (Web) | `mcr.microsoft.com/dotnet/aspnet:8.0` (inferred from Dockerfile) | `src/Web/Dockerfile` |
| Docker base image (PublicApi) | `mcr.microsoft.com/dotnet/aspnet:8.0` (inferred from Dockerfile) | `src/PublicApi/Dockerfile` |
| SQL Server (Docker) | Azure SQL Edge (latest) | `docker-compose.yml` |
| Azure.Identity | 1.10.4 | `Directory.Packages.props` |
| Azure Key Vault Config | 1.3.1 | `Directory.Packages.props` |
| MediatR | 12.0.1 | `Directory.Packages.props` |
| AutoMapper | 12.0.1 | `Directory.Packages.props` |
| Swashbuckle.AspNetCore | 6.5.0 | `Directory.Packages.props` |
| System.IdentityModel.Tokens.Jwt | 7.3.1 | `Directory.Packages.props` |
