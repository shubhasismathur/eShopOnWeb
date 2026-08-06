# Modernization Plan: modnization-plan

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: C# / .NET 8 (net8.0)
- **Framework**: ASP.NET Core 8.0
- **Build Tool**: dotnet SDK 8.0.x
- **Database**: SQL Server (configured via app settings)
- **Key Dependencies**: EF Core, Azure.Identity, Azure Key Vault config provider

---

## Overview

> This migration modernizes eShopOnWeb for Azure-first operations and deployment.
> The application currently runs as a monolithic ASP.NET Core web solution with
> local/developer-centric configuration defaults. The modernized approach will:
>
> - Externalize runtime configuration for cloud-managed operations.
> - Validate migrated behavior through post-migration integration verification.
> - Deploy to Azure Container Apps using existing repository Azure definitions.
>
> The migration follows a phased plan: baseline capture, configuration
> modernization, verification, security remediation, and Azure deployment.

---

## Migration Impact Summary

| Application | Original Service | New Azure Service | Authentication | Comments |
|-------------|------------------|-------------------|----------------|----------|
| eShopOnWeb | Local app config | Azure App Config | Managed Identity | Config externalization |
| eShopOnWeb | Local hosting | Azure Container Apps | Managed Identity | Azure deployment target |

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — use existing infrastructure/configuration already defined in the repository (`infra/`, `azure.yaml`).
- [x] Q: Should the plan include integration testing to verify migrated services? → A: Yes — Real mode using existing repository infrastructure/configuration.
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes (default).
- [x] Q: Which Azure deployment target should the plan use? → A: Azure Container Apps (default).
- [x] Q: Should the plan include containerization (Dockerfile generation)? → A: Not added separately because Azure Container Apps deployment covers containerization.
- [x] Q: Was an assessment report used to map findings to tasks? → A: No assessment report was provided in the current context; tasks are scoped to explicit modernization goals and repository evidence.
