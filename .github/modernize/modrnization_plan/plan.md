# Modernization Plan: eShopOnWeb Azure Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: .NET 8 (net8.0)
- **Framework**: ASP.NET Core 8.0 (MVC + Blazor WebAssembly)
- **Build Tool**: dotnet CLI / MSBuild (Central Package Management via Directory.Packages.props)
- **Database**: SQL Server (via Entity Framework Core 8.0.2 with `Microsoft.EntityFrameworkCore.SqlServer`)
- **Key Dependencies**: Entity Framework Core 8, ASP.NET Core Identity, MediatR, AutoMapper, Blazor WebAssembly, Azure.Identity, Azure.Extensions.AspNetCore.Configuration.Secrets

---

## Overview

> This migration modernizes the eShopOnWeb reference application to run securely on Azure. The application currently runs as an ASP.NET Core 8 MVC/Blazor web store with a local SQL Server database, local connection-string-based authentication, and no cloud-native observability or hardened dependency supply chain. The new architecture will:
>
> - Migrate the SQL Server database connections to **Azure SQL Database** using **Managed Identity** (passwordless authentication), eliminating plaintext connection strings
> - Migrate secrets and configuration to **Azure Key Vault** and **Azure App Configuration**, removing sensitive values from `appsettings.json`
> - Remediate known CVEs in project dependencies to ensure a secure baseline before deployment
> - Deploy the application to **Azure Container Apps** using Bicep-based infrastructure
>
> The migration follows a phased approach: dependency security remediation first, then Azure service integrations (SQL, Key Vault, App Configuration), and finally containerization and deployment to Azure Container Apps.

---

## Migration Impact Summary

| Application  | Original Service           | New Azure Service              | Authentication   | Comments                                      |
|--------------|----------------------------|--------------------------------|------------------|-----------------------------------------------|
| Web / API    | Local SQL Server           | Azure SQL Database             | Managed Identity | EF Core connection migrated to Azure SQL       |
| Web / API    | appsettings.json secrets   | Azure Key Vault (secrets)      | Managed Identity | Connection strings / secrets moved to Key Vault|
| Web / API    | appsettings.json settings  | Azure App Configuration        | Managed Identity | Non-secret config externalized                 |
| Web / API    | Local container / none     | Azure Container Apps           | Managed Identity | New deployment target via Bicep IaC            |

---

## Open Questions & Questionnaire

- [x] Q: Should .NET be upgraded to a newer LTS version? → A: No upgrade required — project targets net8.0, which is current LTS and within mainstream support.
- [x] Q: What is the target Azure deployment platform? → A: Azure Container Apps (default).
- [x] Q: What authentication method should be used for Azure services? → A: Managed Identity (passwordless) for all Azure service connections.
- [x] Q: Should integration tests be generated? → A: No integration testing was explicitly requested — skipped.
- [x] Q: Should infrastructure (Bicep/Terraform) be provisioned? → A: Deployment task requested (deployment to Azure Container Apps) so Bicep IaC will be generated as part of the deployment task.
