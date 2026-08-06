# Modernization Plan: eShopOnWeb Azure Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: C# / .NET 8.0
- **Framework**: ASP.NET Core 8.0 (MVC + Razor Pages + Blazor WebAssembly)
- **Build Tool**: dotnet CLI / MSBuild
- **Database**: SQL Server (Entity Framework Core 8.0 with SqlServer provider)
- **Key Dependencies**: Entity Framework Core 8.0, ASP.NET Core Identity, MediatR, Ardalis.Specification, AutoMapper, Azure.Identity, Azure.Extensions.AspNetCore.Configuration.Secrets, Swashbuckle (PublicApi)

---

## Overview

> This migration modernizes the eShopOnWeb ASP.NET Core 8.0 e-commerce application for cloud-native deployment on Azure. The application currently uses a SQL Server database, local application settings, and runs as a traditional web application. The new architecture will:
>
> - Migrate the SQL Server database to **Azure SQL Database** with Managed Identity passwordless authentication, eliminating hard-coded connection strings
> - Externalize non-secret application configuration to **Azure App Configuration**, centralizing configuration management
> - Secure sensitive credentials (e.g., JWT signing keys, secrets) in **Azure Key Vault**, replacing plain-text secrets in appsettings
> - Deploy the web application to **Azure Container Apps** for scalable, containerized hosting
> - Remediate known CVEs in project dependencies to ensure the application is secure before deployment
>
> The migration follows a phased approach: database migration first, then configuration and secrets externalization, then security remediation, and finally containerized deployment to Azure.

---

## Migration Impact Summary

| Application    | Original Service         | New Azure Service              | Authentication   | Comments                                      |
|----------------|--------------------------|--------------------------------|------------------|-----------------------------------------------|
| eShopOnWeb Web | SQL Server (local/docker)| Azure SQL Database             | Managed Identity | EF Core SqlServer provider, passwordless auth |
| eShopOnWeb Web | appsettings.json         | Azure App Configuration        | Managed Identity | Non-secret settings externalized              |
| eShopOnWeb Web | appsettings.json secrets | Azure Key Vault Secrets        | Managed Identity | JWT keys and sensitive config moved to KV     |
| eShopOnWeb Web | Local hosting            | Azure Container Apps           | Managed Identity | Containerized deployment, new Dockerfile      |

---

## Open Questions & Questionnaire

- [x] Q: What is the target .NET version? → A: .NET 8.0 is current LTS and in support; no upgrade needed unless explicitly requested. No upgrade task added.
- [x] Q: What is the target Azure deployment service? → A: Azure Container Apps (default)
- [x] Q: Should integration testing be included? → A: No integration testing explicitly requested; skipped.
- [x] Q: What authentication method should be used for Azure services? → A: Managed Identity (DefaultAzureCredential) for all Azure services.
- [x] Q: Should infrastructure (Bicep/Terraform) be generated? → A: Not explicitly requested; infrastructure task omitted.
