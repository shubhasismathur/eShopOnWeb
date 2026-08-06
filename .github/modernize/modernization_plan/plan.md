# Modernization Plan: eShopOnWeb Azure Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: .NET 8 (net8.0)
- **Framework**: ASP.NET Core 8.0 with Blazor Server/WebAssembly
- **Build Tool**: dotnet CLI / MSBuild (Central Package Management)
- **Database**: SQL Server (localdb for dev; Azure SQL via Key Vault connection string in prod)
- **Key Dependencies**: Entity Framework Core 8.0, ASP.NET Core Identity, Azure.Identity, Azure.Extensions.AspNetCore.Configuration.Secrets, MediatR, Ardalis.Specification

---

## Overview

> This migration modernizes the eShopOnWeb ASP.NET Core 8 application for cloud-native deployment on Azure. The application currently uses SQL Server with connection strings stored in Azure Key Vault, and local appsettings.json for non-secret configuration. The new architecture will:
>
> - Replace connection-string-based SQL Server access with Azure SQL Database using Managed Identity (passwordless authentication), eliminating credential management risk
> - Externalize non-secret application settings from appsettings.json to Azure App Configuration, centralizing configuration management
> - Remediate known CVEs in third-party dependencies to ensure the application is secure before deployment
> - Deploy the application to Azure Container Apps using containerization and bicep-based infrastructure as code

The migration follows a phased approach: first securing data access with Managed Identity, then externalizing configuration, followed by security remediation, and finally deploying the containerized application to Azure.

---

## Migration Impact Summary

| Application | Original Service         | New Azure Service             | Authentication     | Comments                                      |
|-------------|--------------------------|-------------------------------|--------------------|-----------------------------------------------|
| Web         | SQL Server (conn string) | Azure SQL Database            | Managed Identity   | Migrate CatalogContext and AppIdentityDbContext|
| Web         | appsettings.json         | Azure App Configuration       | Managed Identity   | Externalize non-secret settings               |
| Web         | N/A                      | Azure Container Apps          | Managed Identity   | Containerize and deploy via ACA               |

---

## Open Questions & Questionnaire

- [x] Q: What .NET upgrade is needed? → A: .NET 8 is current LTS and in mainstream support — no upgrade required.
- [x] Q: What is the target deployment platform? → A: Azure Container Apps (default).
- [x] Q: What authentication method for Azure services? → A: Managed Identity (DefaultAzureCredential).
- [x] Q: Should integration testing be included? → A: No explicit request — skipped.
- [x] Q: Should infrastructure provisioning be included? → A: No explicit request — skipped.
