# Configuration & Externalized Settings Inventory

Configuration is split across .NET appsettings files, environment variables, and centralized package/runtime settings, with optional cloud secret integration for production-like environments.

## Configuration Sources

| Source | Type | Path/Location | Notes |
|---|---|---|---|
| Web settings | JSON config | `src/Web/appsettings.json` | Base URLs, DB connection strings, logging defaults |
| Web Docker settings | JSON config | `src/Web/appsettings.Docker.json` | Container/runtime overrides |
| Public API settings | JSON config | `src/PublicApi/appsettings.json`, `appsettings.test.json` | API base URLs, test configuration |
| Blazor Admin settings | JSON config | `src/BlazorAdmin/wwwroot/appsettings.json` | Client base URL settings |
| Environment variables | Runtime source | Process environment | Added through `AddEnvironmentVariables()` |
| Azure Key Vault | External secret source | `AZURE_KEY_VAULT_ENDPOINT` | Enabled in non-development Web startup |
| Central package versions | Build config | `Directory.Packages.props` | Shared package and framework version control |
| SDK pinning | Build config | `global.json` | Locks .NET SDK major train |

## Build Profiles

| Profile | Activation | Purpose | Key Dependencies/Plugins |
|---|---|---|---|
| Debug | Default local build | Developer diagnostics and non-optimized builds | Standard SDK pipeline |
| Release | `-c Release` | Optimized production build | Includes `BuildBundlerMinifier` conditionally |
| Docker environment mode | `EnvironmentName == Docker` | Uses development-like DB wiring for container workflows | SQL Server/local config path |

## Runtime Profiles

| Profile | Activation Method | Config Files | Key Overrides |
|---|---|---|---|
| Development | `ASPNETCORE_ENVIRONMENT=Development` | appsettings + user env | Developer exception page, migrations endpoint |
| Docker | `EnvironmentName=Docker` | appsettings.Docker.json + env vars | Docker base URL and infra wiring |
| Production-like | Non-development branch | appsettings + KeyVault + env vars | KeyVault-backed SQL connection resolution |

## Properties Inventory

| Property Key | Default | Profiles | Source |
|---|---|---|---|
| `baseUrls:apiBase` | `https://localhost:5099/api/` | Web/PublicApi local | appsettings.json |
| `baseUrls:webBase` | `https://localhost:44315/` | Web/PublicApi local | appsettings.json |
| `ConnectionStrings:CatalogConnection` | localdb SQL Server | Development/Docker | appsettings.json |
| `ConnectionStrings:IdentityConnection` | localdb SQL Server | Development/Docker | appsettings.json |
| `CatalogBaseUrl` | empty | All | appsettings.json + env override |
| `UseOnlyInMemoryDatabase` | false (implicit) | Optional override | environment/app config |
| `AZURE_KEY_VAULT_ENDPOINT` | unset | Production-like | environment variable |
| `AZURE_SQL_CATALOG_CONNECTION_STRING_KEY` | unset | Production-like | environment variable |
| `AZURE_SQL_IDENTITY_CONNECTION_STRING_KEY` | unset | Production-like | environment variable |

## Startup Parameters & Resource Requirements

| Service | JVM/Runtime Options | Memory | Instance Count |
|---|---|---|---|
| Web | .NET runtime defaults, ASP.NET Core env variables | Not explicitly configured in repo | Not specified |
| PublicApi | .NET runtime defaults, ASP.NET Core env variables | Not explicitly configured in repo | Not specified |
| BlazorAdmin | Browser-hosted wasm + server host defaults | Browser/device dependent | Not specified |

## Startup Dependency Chain

1. Web/PublicApi host starts and loads appsettings + environment variables.
2. In production-like mode, Web resolves KeyVault secrets before DB context registration.
3. Database contexts are configured (SQL Server or in-memory based on environment flag).
4. On startup, both Web and PublicApi run seed routines for catalog and identity stores.
5. HTTP endpoints become available after middleware and routing setup completes.

## Secrets & Sensitive Configuration

| Secret Reference | Type | Storage (masked) |
|---|---|---|
| `ConnectionStrings:CatalogConnection` | Database connection string | appsettings (`[MASKED]`) or KeyVault-backed value |
| `ConnectionStrings:IdentityConnection` | Database connection string | appsettings (`[MASKED]`) or KeyVault-backed value |
| `AZURE_KEY_VAULT_ENDPOINT` | Secret store endpoint | Environment variable |
| `AZURE_SQL_*_CONNECTION_STRING_KEY` | Key indirection for secret lookup | Environment variable |
| `AuthorizationConstants.JWT_SECRET_KEY` | JWT signing secret constant reference | Source constant (`[MASKED]`) |

### Secrets Provisioning Workflow

Runtime secrets are sourced from appsettings for local use and from environment-driven Azure Key Vault references in non-development environments. The process is: environment variables provide Key Vault endpoint and key names, startup resolves secret values via managed identity credentials, then DB contexts and security middleware consume those resolved values.

## Feature Flags

| Flag Name | Default | Controlled By |
|---|---|---|
| `UseOnlyInMemoryDatabase` | false | App configuration / environment variable |
| `CatalogBaseUrl` behavior toggle | empty (disabled) | App configuration |

## Framework & Runtime Versions

| Component | Version | Source |
|---|---|---|
| .NET SDK | 8.0.x | `global.json` |
| Target framework | net8.0 | `Directory.Packages.props` |
| ASP.NET Core packages | 8.0.2 | `Directory.Packages.props` |
| EF Core packages | 8.0.2 | `Directory.Packages.props` |
| Swashbuckle.AspNetCore | 6.5.0 | `Directory.Packages.props` |
| Azure.Identity | 1.10.4 | `Directory.Packages.props` |
