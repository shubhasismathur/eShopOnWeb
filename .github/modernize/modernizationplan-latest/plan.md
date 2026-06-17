# Modernization Plan: eShopOnWeb Azure Modernization

**Project**: eShopOnWeb

---

## Technical Framework

- **Language**: C# / .NET 8.0
- **Framework**: ASP.NET Core 8.0 with Blazor WebAssembly
- **Build Tool**: .NET SDK 8.0 (MSBuild)
- **Database**: SQL Server (localdb for development), Azure SQL Database (production)
- **Key Dependencies**: Entity Framework Core 8.0, ASP.NET Core Identity, Azure.Identity 1.10.4, Azure.Extensions.AspNetCore.Configuration.Secrets 1.3.1, Ardalis.Specification 7.0.0

---

## Overview

> This migration modernizes the eShopOnWeb ASP.NET Core 8.0 e-commerce application to use Azure managed services with passwordless authentication. The application currently authenticates to SQL Server using password-based connection strings stored in Azure Key Vault. The new architecture will:
>
> - Replace password-based SQL Server authentication with Azure Managed Identity for secure, passwordless database access, eliminating stored credentials from the Key Vault
> - Ensure all project dependencies are free of known CVE vulnerabilities before deployment
>
> The migration follows a phased approach: first modernizing data-layer authentication to use Managed Identity, then performing a security scan and CVE remediation across all project dependencies.

---

## Migration Impact Summary

| Application     | Original Service                        | New Azure Service      | Authentication   | Comments                                                          |
|-----------------|-----------------------------------------|------------------------|------------------|-------------------------------------------------------------------|
| eShopOnWeb Web  | SQL Server (password connection string) | Azure SQL Database     | Managed Identity | Migrate CatalogConnection and IdentityConnection to use passwordless Managed Identity |

---

## Modernization Tasks

### Task 1 — Migrate SQL Server to Azure SQL Database with Managed Identity

Migrate the eShopOnWeb application's two Entity Framework Core database contexts (`CatalogContext` and `AppIdentityDbContext`) from password-based SQL Server connection strings to passwordless Azure SQL Database access using Azure Managed Identity. This eliminates stored credentials and aligns with Azure security best practices.

### Task 2 — Security / CVE Remediation

Scan all project dependencies for known CVEs and remediate any identified vulnerabilities to ensure the application is secure before deployment. Upgrade vulnerable packages to the minimum patched version, document any breaking changes, and verify that the project builds and all tests pass after remediation.

---

## Open Questions & Questionnaire

- [x] Q: Should the plan include environment/infrastructure provisioning? → A: No — use infrastructure already defined in the repository (`infra/main.bicep`, `azure.yaml`); the Azure App Service, Azure SQL, and Key Vault are already provisioned via the existing Bicep IaC.
- [x] Q: Should the plan include integration testing? → A: No — skip integration testing; user did not explicitly request it.
- [x] Q: Should the plan include a security scan and CVE remediation task? → A: Yes — include security/CVE remediation (default).
- [x] Q: Which Azure deployment target should the plan use? → A: No deployment — migration only; `azure.yaml` already configures App Service deployment.
