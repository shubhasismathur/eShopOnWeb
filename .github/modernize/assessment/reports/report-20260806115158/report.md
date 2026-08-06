# ApplicationCore

## Summary

| Metric | Value |
|--------|-------|
| Total Issues | 24 |
| Mandatory Blockers | 8 |
| Potential Issues | 11 |

## Component Information

| Property | Value |
|----------|-------|
| Language | C# |
| Frameworks | net8.0 |
| Build tools | MSBuild |

## Cloud Readiness Issues

| Issue Name | Criticality | Story Points | Occurrences |
|------------|-------------|--------------|-------------|
| Local application configuration detected | Potential | 1 | [15](#Local_application_configuration_detected) |
| Hardcoded URLs detected | Potential | 1 | [13](#Hardcoded_URLs_detected) |
| Access to external resources via HTTP is detected | Potential | 3 | [11](#Access_to_external_resources_via_HTTP_is_detected) |
| Certificate management dependency detected | Potential | 5 | [8](#Certificate_management_dependency_detected) |
| Connection string is detected | Potential | 3 | [8](#Connection_string_is_detected) |
| Data caching is detected | Potential | 3 | [2](#Data_caching_is_detected) |
| Environment variables dependency detected | Potential | 3 | [1](#Environment_variables_dependency_detected) |
| Hardcoded sensitive data detected | Optional | 3 | [29](#Hardcoded_sensitive_data_detected) |
| Synchronous API usage detected | Optional | 1 | [9](#Synchronous_API_usage_detected) |
| Static content detected | Optional | 3 | [2](#Static_content_detected) |

### Issue Details

<details id="Local_application_configuration_detected">
<summary><b>Local application configuration detected</b> — affected files</summary>

- `src/BlazorAdmin/wwwroot/appsettings.Docker.json`
- `src/BlazorAdmin/wwwroot/appsettings.Development.json`
- `src/BlazorAdmin/wwwroot/appsettings.json`
- `src/PublicApi/appsettings.Docker.json`
- `src/PublicApi/appsettings.Docker.json`
- `src/PublicApi/appsettings.Development.json`
- `src/PublicApi/appsettings.json`
- `src/PublicApi/appsettings.json`
- `src/PublicApi/appsettings.json`
- `src/Web/appsettings.Docker.json`
- `src/Web/appsettings.Docker.json`
- `src/Web/appsettings.Development.json`
- `src/Web/appsettings.json`
- `src/Web/appsettings.json`
- `src/Web/appsettings.json`

</details>

<details id="Hardcoded_URLs_detected">
<summary><b>Hardcoded URLs detected</b> — affected files</summary>

- `src/ApplicationCore/Services/UriComposer.cs (line 12)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 86)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 87)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 88)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 89)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 90)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 91)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 92)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 93)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 94)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 95)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 96)`
- `src/Infrastructure/Data/CatalogContextSeed.cs (line 97)`

</details>

<details id="Access_to_external_resources_via_HTTP_is_detected">
<summary><b>Access to external resources via HTTP is detected</b> — affected files</summary>

- `src/BlazorAdmin/CustomAuthStateProvider.cs (line 17)`
- `src/BlazorAdmin/CustomAuthStateProvider.cs (line 23)`
- `src/BlazorAdmin/Program.cs (line 22)`
- `src/BlazorAdmin/Services/CatalogLookupDataService.cs (line 21)`
- `src/BlazorAdmin/Services/CatalogLookupDataService.cs (line 25)`
- `src/BlazorAdmin/Services/HttpService.cs (line 12)`
- `src/BlazorAdmin/Services/HttpService.cs (line 17)`
- `src/Web/Program.cs (line 101)`
- `src/Web/Program.cs (line 101)`
- `src/Web/HealthChecks/ApiHealthCheck.cs (line 23)`
- `src/Web/HealthChecks/HomePageHealthCheck.cs (line 24)`

</details>

<details id="Certificate_management_dependency_detected">
<summary><b>Certificate management dependency detected</b> — affected files</summary>

- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 42)`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 36)`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 36)`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 40)`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 40)`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs (line 40)`
- `src/PublicApi/Program.cs (line 62)`
- `src/PublicApi/Program.cs (line 65)`

</details>

<details id="Connection_string_is_detected">
<summary><b>Connection string is detected</b> — affected files</summary>

- `src/PublicApi/appsettings.Docker.json`
- `src/PublicApi/appsettings.Docker.json`
- `src/PublicApi/appsettings.json`
- `src/PublicApi/appsettings.json`
- `src/Web/appsettings.Docker.json`
- `src/Web/appsettings.Docker.json`
- `src/Web/appsettings.json`
- `src/Web/appsettings.json`

</details>

<details id="Data_caching_is_detected">
<summary><b>Data caching is detected</b> — affected files</summary>

- `src/PublicApi/Program.cs (line 51)`
- `src/Web/Program.cs (line 65)`

</details>

<details id="Environment_variables_dependency_detected">
<summary><b>Environment variables dependency detected</b> — affected files</summary>

- `src/PublicApi/Properties/launchSettings.json`

</details>

<details id="Hardcoded_sensitive_data_detected">
<summary><b>Hardcoded sensitive data detected</b> — affected files</summary>

- `src/ApplicationCore/Constants/AuthorizationConstants.cs (line 10)`
- `src/Infrastructure/Identity/Migrations/20201202111612_InitialIdentityModel.Designer.cs (line 187)`
- `src/Infrastructure/Identity/Migrations/AppIdentityDbContextModelSnapshot.cs (line 185)`
- `src/Web/Controllers/ManageController.cs (line 178)`
- `src/Web/Controllers/ManageController.cs (line 25)`
- `src/Web/Controllers/ManageController.cs (line 179)`
- `src/Web/Controllers/ManageController.cs (line 227)`
- `src/Web/Views/Manage/ManageNavPages.cs (line 12)`
- `src/Web/ViewModels/Account/RegisterViewModel.cs (line 18)`
- `src/Web/ViewModels/Account/RegisterViewModel.cs (line 14)`
- `src/Web/ViewModels/Account/RegisterViewModel.cs (line 19)`
- `src/Web/ViewModels/Account/RegisterViewModel.cs (line 19)`
- `src/Web/ViewModels/Account/ResetPasswordViewModel.cs (line 16)`
- `src/Web/ViewModels/Account/ResetPasswordViewModel.cs (line 17)`
- `src/Web/ViewModels/Account/ResetPasswordViewModel.cs (line 17)`
- `src/Web/ViewModels/Manage/SetPasswordViewModel.cs (line 13)`
- `src/Web/ViewModels/Manage/SetPasswordViewModel.cs (line 9)`
- `src/Web/ViewModels/Manage/SetPasswordViewModel.cs (line 14)`
- `src/Web/ViewModels/Manage/SetPasswordViewModel.cs (line 14)`
- `src/Web/ViewModels/Manage/ChangePasswordViewModel.cs (line 18)`
- `src/Web/ViewModels/Manage/ChangePasswordViewModel.cs (line 8)`
- `src/Web/ViewModels/Manage/ChangePasswordViewModel.cs (line 14)`
- `src/Web/ViewModels/Manage/ChangePasswordViewModel.cs (line 19)`
- `src/Web/ViewModels/Manage/ChangePasswordViewModel.cs (line 19)`
- `src/Web/Areas/Identity/Pages/Account/Register.cshtml.cs (line 74)`
- `src/Web/Areas/Identity/Pages/Account/Register.cshtml.cs (line 55)`
- `src/Web/Areas/Identity/Pages/Account/Register.cshtml.cs (line 51)`
- `src/Web/Areas/Identity/Pages/Account/Register.cshtml.cs (line 56)`
- `src/Web/Areas/Identity/Pages/Account/Register.cshtml.cs (line 56)`

</details>

<details id="Synchronous_API_usage_detected">
<summary><b>Synchronous API usage detected</b> — affected files</summary>

- `src/BlazorAdmin/Services/CatalogItemService.cs (line 50)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 66)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 85)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 52)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 68)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 87)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 51)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 67)`
- `src/BlazorAdmin/Services/CatalogItemService.cs (line 86)`

</details>

<details id="Static_content_detected">
<summary><b>Static content detected</b> — affected files</summary>

- `src/BlazorAdmin/BlazorAdmin.csproj`
- `src/Web/Web.csproj`

</details>

## Security Issues

> **Note:** These issues were generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

| Issue Name | Criticality | Story Points | Files |
|------------|-------------|--------------|-------|
| CWE-434: Unrestricted Upload of File with Dangerous Type | Mandatory | 8 | [3](#CWE-434_Unrestricted_Upload_of_File_with_Dangerous_Type) |
| CVE-2026-32933: AutoMapper Vulnerable to Denial of Service (DoS) via Uncontrolled Recursion | Mandatory | 1 | [1](#CVE-2026-32933_AutoMapper_Vulnerable_to_Denial_of_Service_DoS_via_Uncontrolled_Recursion) |
| CVE-2024-43483: Microsoft Security Advisory CVE-2024-43483 \| .NET Denial of Service Vulnerability | Mandatory | 1 | [1](#CVE-2024-43483_Microsoft_Security_Advisory_CVE-2024-43483_NET_Denial_of_Service_Vulnerability) |
| CVE-2024-0057: NuGet Client Security Feature Bypass Vulnerability  | Mandatory | 1 | [1](#CVE-2024-0057_NuGet_Client_Security_Feature_Bypass_Vulnerability) |
| CVE-2023-29337: NuGet Client Remote Code Execution Vulnerability | Mandatory | 1 | [1](#CVE-2023-29337_NuGet_Client_Remote_Code_Execution_Vulnerability) |
| CVE-2024-38095: Microsoft Security Advisory CVE-2024-38095 \| .NET Denial of Service Vulnerability | Mandatory | 1 | [1](#CVE-2024-38095_Microsoft_Security_Advisory_CVE-2024-38095_NET_Denial_of_Service_Vulnerability) |
| CVE-2024-43485: Microsoft Security Advisory CVE-2024-43485 \| .NET Denial of Service Vulnerability | Mandatory | 1 | [1](#CVE-2024-43485_Microsoft_Security_Advisory_CVE-2024-43485_NET_Denial_of_Service_Vulnerability) |
| CVE-2024-30105: Microsoft Security Advisory CVE-2024-30105 \| .NET Denial of Service Vulnerability | Mandatory | 1 | [1](#CVE-2024-30105_Microsoft_Security_Advisory_CVE-2024-30105_NET_Denial_of_Service_Vulnerability) |
| CWE-321: Use of Hard-coded Cryptographic Key | Potential | 5 | [2](#CWE-321_Use_of_Hard-coded_Cryptographic_Key) |
| CWE-772: Missing Release of Resource after Effective Lifetime | Potential | 3 | [1](#CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime) |
| CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime | Potential | 3 | [1](#CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime) |
| CWE-778: Insufficient Logging | Potential | 3 | [1](#CWE-778_Insufficient_Logging) |
| CWE-259: Use of Hard-coded Password | Optional | 5 | [2](#CWE-259_Use_of_Hard-coded_Password) |
| CWE-798: Use of Hard-coded Credentials | Optional | 5 | [5](#CWE-798_Use_of_Hard-coded_Credentials) |

### Security Issue Details

<details id="CWE-434_Unrestricted_Upload_of_File_with_Dangerous_Type">
<summary><b>CWE-434: Unrestricted Upload of File with Dangerous Type</b> — affected files</summary>

- `src/PublicApi/ImageValidators.cs`
- `src/PublicApi/CatalogItemEndpoints/CreateCatalogItemEndpoint.cs`
- `src/BlazorShared/Models/CatalogItem.cs`

</details>

<details id="CVE-2026-32933_AutoMapper_Vulnerable_to_Denial_of_Service_DoS_via_Uncontrolled_Recursion">
<summary><b>CVE-2026-32933: AutoMapper Vulnerable to Denial of Service (DoS) via Uncontrolled Recursion</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2024-43483_Microsoft_Security_Advisory_CVE-2024-43483_NET_Denial_of_Service_Vulnerability">
<summary><b>CVE-2024-43483: Microsoft Security Advisory CVE-2024-43483 | .NET Denial of Service Vulnerability</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2024-0057_NuGet_Client_Security_Feature_Bypass_Vulnerability">
<summary><b>CVE-2024-0057: NuGet Client Security Feature Bypass Vulnerability </b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2023-29337_NuGet_Client_Remote_Code_Execution_Vulnerability">
<summary><b>CVE-2023-29337: NuGet Client Remote Code Execution Vulnerability</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2024-38095_Microsoft_Security_Advisory_CVE-2024-38095_NET_Denial_of_Service_Vulnerability">
<summary><b>CVE-2024-38095: Microsoft Security Advisory CVE-2024-38095 | .NET Denial of Service Vulnerability</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2024-43485_Microsoft_Security_Advisory_CVE-2024-43485_NET_Denial_of_Service_Vulnerability">
<summary><b>CVE-2024-43485: Microsoft Security Advisory CVE-2024-43485 | .NET Denial of Service Vulnerability</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CVE-2024-30105_Microsoft_Security_Advisory_CVE-2024-30105_NET_Denial_of_Service_Vulnerability">
<summary><b>CVE-2024-30105: Microsoft Security Advisory CVE-2024-30105 | .NET Denial of Service Vulnerability</b> — affected files</summary>

- `Directory.Packages.props`

</details>

<details id="CWE-321_Use_of_Hard-coded_Cryptographic_Key">
<summary><b>CWE-321: Use of Hard-coded Cryptographic Key</b> — affected files</summary>

- `src/ApplicationCore/Constants/AuthorizationConstants.cs`
- `src/Infrastructure/Identity/IdentityTokenClaimService.cs`

</details>

<details id="CWE-772_Missing_Release_of_Resource_after_Effective_Lifetime">
<summary><b>CWE-772: Missing Release of Resource after Effective Lifetime</b> — affected files</summary>

- `src/Web/HealthChecks/ApiHealthCheck.cs`

</details>

<details id="CWE-775_Missing_Release_of_File_Descriptor_or_Handle_after_Effective_Lifetime">
<summary><b>CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime</b> — affected files</summary>

- `src/Web/HealthChecks/ApiHealthCheck.cs`

</details>

<details id="CWE-778_Insufficient_Logging">
<summary><b>CWE-778: Insufficient Logging</b> — affected files</summary>

- `src/PublicApi/AuthEndpoints/AuthenticateEndpoint.cs`

</details>

<details id="CWE-259_Use_of_Hard-coded_Password">
<summary><b>CWE-259: Use of Hard-coded Password</b> — affected files</summary>

- `src/ApplicationCore/Constants/AuthorizationConstants.cs`
- `docker-compose.yml`

</details>

<details id="CWE-798_Use_of_Hard-coded_Credentials">
<summary><b>CWE-798: Use of Hard-coded Credentials</b> — affected files</summary>

- `src/ApplicationCore/Constants/AuthorizationConstants.cs`
- `src/ApplicationCore/Constants/AuthorizationConstants.cs`
- `docker-compose.yml`
- `src/Web/appsettings.Docker.json`
- `src/PublicApi/appsettings.Docker.json`

</details>

---

## Codebase Insights

> **Note:** These documents are generated by AI and may contain inaccuracies or incomplete information. Please review carefully.

1. **[Architecture Diagram](facts/architecture-diagram.md)** — Understand the big picture: system layers and component relationships
2. **[Dependency Map](facts/dependency-map.md)** — Know what the project depends on and where the risks are
3. **[API & Service Contracts](facts/api-service-contracts.md)** — See how services communicate and what contracts they expose
4. **[Data Architecture](facts/data-architecture.md)** — Explore data models, storage, and data flow patterns
5. **[Configuration Inventory](facts/configuration-inventory.md)** — Review how the application is configured across environments
6. **[Business Workflows](facts/business-workflows.md)** — Trace end-to-end business processes and domain logic

[Share feedback](https://aka.ms/ghcp-appmod/feedback)
