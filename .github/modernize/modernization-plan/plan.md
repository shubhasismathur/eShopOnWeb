# Modernization Plan: eShopOnWeb Azure Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: .NET 8 (net8.0)
- **Framework**: ASP.NET Core 8.0, Blazor WebAssembly 8.0
- **Build Tool**: dotnet SDK 8.0 / MSBuild (Central Package Management)
- **Database**: SQL Server (via Entity Framework Core 8.0 / Microsoft.EntityFrameworkCore.SqlServer)
- **Key Dependencies**: Entity Framework Core 8.0, MediatR 12, AutoMapper, Ardalis.Specification, Azure.Identity, Swashbuckle/OpenAPI, ASP.NET Core Identity

---

## Overview

> This migration modernizes the eShopOnWeb reference application for cloud-native deployment on Azure. The application currently runs as a monolithic ASP.NET Core 8 web application with a SQL Server database, using local configuration files for settings and connection strings. The new architecture will:
>
> - Migrate the SQL Server database to **Azure SQL Database** with Managed Identity authentication, eliminating the need for passwords in connection strings
> - Migrate secrets (keys, tokens) to **Azure Key Vault** for secure, centralized secret management
> - Externalize non-secret application settings to **Azure App Configuration** for centralized configuration management
> - Add **OpenTelemetry** instrumentation with Azure Monitor for observability (tracing, metrics, logging)
> - Containerize and deploy the web application and API to **Azure Container Apps** for scalable, managed hosting
> - Remediate known CVEs in project dependencies to ensure the application is secure before deployment
> - Upgrade the runtime to **.NET 10 (latest LTS)** to ensure long-term support
>
> The migration follows a phased approach: upgrade the runtime first, then migrate platform services (database, secrets, configuration, observability), containerize, and finally deploy.

---

## Migration Impact Summary

| Application     | Original Service          | New Azure Service             | Authentication     | Comments                              |
|-----------------|--------------------------|-------------------------------|--------------------|---------------------------------------|
| Web / PublicApi | SQL Server (local)       | Azure SQL Database            | Managed Identity   | EF Core provider update               |
| Web / PublicApi | Local appsettings.json   | Azure App Configuration       | Managed Identity   | Non-secret settings externalised      |
| Web / PublicApi | Local secrets / env vars | Azure Key Vault               | Managed Identity   | JWT signing keys, connection strings  |
| Web / PublicApi | Console logging only     | Azure Monitor (OpenTelemetry) | Managed Identity   | Traces, metrics, structured logs      |
| Web / PublicApi | Local process            | Azure Container Apps          | Managed Identity   | Containerized deployment              |

---

## Open Questions & Questionnaire

- [x] Q: What is the target .NET version? → A: Upgrade to .NET 10 (latest LTS) as current .NET 8 is approaching EOL and the user requested modernization
- [x] Q: What authentication method should be used for Azure services? → A: Managed Identity (DefaultAzureCredential) for all Azure services
- [x] Q: What is the target deployment platform? → A: Azure Container Apps (default)
- [x] Q: Should integration tests be included? → A: No — skipped (not explicitly requested by user)
- [x] Q: Should infrastructure (Bicep/Terraform) be generated? → A: No — not explicitly requested by user
