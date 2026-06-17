# Modernization Plan: eShopOnWeb Azure modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: C# / .NET 8
- **Framework**: ASP.NET Core 8.0 with Blazor WebAssembly and Minimal APIs
- **Build Tool**: MSBuild / dotnet CLI
- **Database**: SQL Server / LocalDB locally, Azure SQL in the production path
- **Key Dependencies**: Entity Framework Core 8, ASP.NET Core Identity, Azure.Identity, Swashbuckle

---

## Overview

> This migration modernizes the eShopOnWeb applications for consistent Azure-hosted operation. The application currently relies on environment-specific appsettings values, local SQL-oriented defaults, and several hardcoded URL and secret patterns that are suitable for local development but not ideal for cloud deployment. The new architecture will:
>
> - externalize deploy-time settings and service endpoints so environments can be configured without code changes
> - align secret handling and database connectivity with Azure-managed identity and hosted secret storage
> - use the repository's existing Azure deployment assets to deliver the modernized application through a repeatable App Service workflow
>
> The migration follows a phased approach that first externalizes configuration and Azure resource access, then remediates security findings, and finally deploys through the repository's existing Azure path.

---

## Migration Impact Summary

| Application | Original Service | New Azure Service | Authentication | Comments |
|-------------|------------------|-------------------|----------------|----------|
| Web | Local appsettings + SQL Server | Azure App Service + Azure SQL + Key Vault | Managed identity | Reuse existing azd/Bicep deployment path |
| PublicApi | Local appsettings + SQL Server | Azure App Service-hosted API + Azure SQL | Managed identity | Externalize base URLs and secret-backed settings |
| BlazorAdmin | Static appsettings files | App Service-hosted client configuration | Existing app auth | Consume externalized API and web endpoints |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — use the App Service, SQL, and Key Vault Bicep assets already present in the repository.
- [x] Q: Should the plan include integration testing to verify migrated services? → A: No separate integration-test task was added because the request did not explicitly ask for one; existing solution tests remain the baseline validation path.
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes.
- [x] Q: Which Azure deployment target should the plan use? → A: Azure App Service, aligned with `azure.yaml` and `infra/main.bicep`.
- [x] Q: Should the plan include containerization? → A: No separate containerization task; deployment will use the existing App Service path and current Docker assets only if needed.
- [x] Q: Is a .NET runtime upgrade required? → A: No — the repository already targets supported `net8.0` and the request did not ask for a framework upgrade.
