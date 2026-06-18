# .NET Upgrade Plan: net8.0 → net10.0

## Overview

Upgrade the **eShopOnWeb** solution from **.NET 8.0** to **.NET 10.0** (latest LTS). The user explicitly requested an upgrade to the latest LTS version. While .NET 8.0 is currently in support, .NET 10.0 is the current latest LTS, providing longer support lifetime, performance improvements, and access to the newest platform APIs.

## Source Version

- **Current framework**: `net8.0`
- **SDK version**: `8.0.x` (defined in `global.json`)

## Target Version

- **Target framework**: `net10.0`
- **SDK version**: `10.0.x`

## Projects in Solution

### Source Projects
| Project | Path |
|---------|------|
| Web (ASP.NET Core MVC + Blazor Host) | `src/Web/Web.csproj` |
| PublicApi (ASP.NET Core Web API) | `src/PublicApi/PublicApi.csproj` |
| ApplicationCore (Class Library) | `src/ApplicationCore/ApplicationCore.csproj` |
| Infrastructure (Class Library) | `src/Infrastructure/Infrastructure.csproj` |
| BlazorAdmin (Blazor WebAssembly) | `src/BlazorAdmin/BlazorAdmin.csproj` |
| BlazorShared (Blazor Shared Library) | `src/BlazorShared/BlazorShared.csproj` |

### Test Projects
| Project | Path |
|---------|------|
| UnitTests | `tests/UnitTests/UnitTests.csproj` |
| IntegrationTests | `tests/IntegrationTests/IntegrationTests.csproj` |
| FunctionalTests | `tests/FunctionalTests/FunctionalTests.csproj` |
| PublicApiIntegrationTests | `tests/PublicApiIntegrationTests/PublicApiIntegrationTests.csproj` |

## What the Upgrade Entails

1. **Target Framework Moniker (TFM) update**: Change `net8.0` → `net10.0` in `Directory.Packages.props` (centrally managed) and any individual `.csproj` files where TFM is specified.
2. **SDK version update**: Update `global.json` from `8.0.x` to `10.0.x`.
3. **NuGet package updates**: Update all package versions that depend on the ASP.NET Core / .NET version (e.g., `Microsoft.AspNetCore.*`, `Microsoft.EntityFrameworkCore.*`, `Microsoft.Extensions.*`) to their .NET 10-compatible versions in `Directory.Packages.props`.
4. **API compatibility fixes**: Address any breaking changes or deprecated APIs introduced between .NET 8 and .NET 10.
5. **Blazor WebAssembly compatibility**: Verify and update Blazor WebAssembly packages to net10.0-compatible versions.
6. **Build and test validation**: Ensure all projects compile and all unit/integration/functional tests pass.

## Tasks

- `001-upgrade-dotnet-to-net10`: Upgrade all projects from net8.0 to net10.0
