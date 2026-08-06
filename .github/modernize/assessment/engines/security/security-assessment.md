# Security Assessment Report

**Generated:** 2026-08-06T12:05:42.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 14 |
| CVE Vulnerabilities | 7 |
| CWE Vulnerabilities | 7 |
| Total Rules Assessed | 59 |
| Rules Passed | 52 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 8 |
| optional | 2 |
| potential | 4 |

## CVE Findings (Dependency Vulnerabilities)

### CVE-2026-32933: AutoMapper Vulnerable to Denial of Service (DoS) via Uncontrolled Recursion
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2026-32933](https://github.com/advisories/GHSA-rvv3-g6hj-g44x): AutoMapper Vulnerable to Denial of Service (DoS) via Uncontrolled Recursion

Severity: HIGH

Affected dependencies:
  - AutoMapper@12.0.1 (declared at Directory.Packages.props)
  - AutoMapper@12.0.1 (declared at Directory.Packages.props)

Vulnerable version range: < 15.1.1
Recommended fix: Upgrade to patched version 15.1.1 or later

### CVE-2024-43483: Microsoft Security Advisory CVE-2024-43483 | .NET Denial of Service Vulnerabilit
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2024-43483](https://github.com/advisories/GHSA-qj66-m88j-hmgj): Microsoft Security Advisory CVE-2024-43483 | .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - Microsoft.Extensions.Caching.Memory@8.0.0 (declared at Directory.Packages.props)
  - Microsoft.Extensions.Caching.Memory@8.0.0 (declared at Directory.Packages.props)
  - Microsoft.Extensions.Caching.Memory@8.0.0 (declared at Directory.Packages.props)

Vulnerable version range: >= 6.0.0-preview.1.21102.12, <= 6.0.1
Recommended fix: Upgrade to patched version 6.0.2 or later

### CVE-2024-0057: NuGet Client Security Feature Bypass Vulnerability 
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2024-0057](https://github.com/advisories/GHSA-68w7-72jg-6qpp): NuGet Client Security Feature Bypass Vulnerability 

Severity: CRITICAL

Affected dependencies:
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Packaging@6.3.1 (declared at Directory.Packages.props)

Vulnerable version range: = 6.8.0
Recommended fix: Upgrade to patched version 6.8.1 or later

### CVE-2023-29337: NuGet Client Remote Code Execution Vulnerability
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2023-29337](https://github.com/advisories/GHSA-6qmf-mmc7-6c2p): NuGet Client Remote Code Execution Vulnerability

Severity: HIGH

Affected dependencies:
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Common@6.3.1 (declared at Directory.Packages.props)
  - NuGet.Protocol@6.3.1

### CVE-2024-38095: Microsoft Security Advisory CVE-2024-38095 | .NET Denial of Service Vulnerabilit
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2024-38095](https://github.com/advisories/GHSA-447r-wph3-92pm): Microsoft Security Advisory CVE-2024-38095 | .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - System.Formats.Asn1@5.0.0 (declared at Directory.Packages.props)
  - System.Formats.Asn1@5.0.0 (declared at Directory.Packages.props)

Vulnerable version range: >= 7.0.0-preview.1.22076.8, < 8.0.1
Recommended fix: Upgrade to patched version 8.0.1 or later

### CVE-2024-43485: Microsoft Security Advisory CVE-2024-43485 | .NET Denial of Service Vulnerabilit
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2024-43485](https://github.com/advisories/GHSA-8g4q-xg66-9fp4): Microsoft Security Advisory CVE-2024-43485 | .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - System.Text.Json@8.0.3 (declared at Directory.Packages.props)
  - System.Text.Json@8.0.3 (declared at Directory.Packages.props)

Vulnerable version range: >= 6.0.0, <= 6.0.9
Recommended fix: Upgrade to patched version 6.0.10 or later

### CVE-2024-30105: Microsoft Security Advisory CVE-2024-30105 | .NET Denial of Service Vulnerabilit
- **Severity:** mandatory
- **Story Points:** 1
- **Files:** Directory.Packages.props

[CVE-2024-30105](https://github.com/advisories/GHSA-hh2w-p6rv-4g7w): Microsoft Security Advisory CVE-2024-30105 | .NET Denial of Service Vulnerability

Severity: HIGH

Affected dependencies:
  - System.Text.Json@8.0.3 (declared at Directory.Packages.props)

Vulnerable version range: >= 7.0.0, < 8.0.4
Recommended fix: Upgrade to patched version 8.0.4 or later

## CWE Findings (Code-Level Vulnerabilities)

### CWE-772: Missing Release of Resource after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/Web/HealthChecks/ApiHealthCheck.cs

In ApiHealthCheck.CheckHealthAsync(), a new HttpClient instance is created with 'var client = new HttpClient()' but is never disposed. HttpClient implements IDisposable, and creating a new instance per health check call without disposal leads to socket exhaustion over time. The instance should be wrapped in a 'using' block or, preferably, injected via IHttpClientFactory.

### CWE-775: Missing Release of File Descriptor or Handle after Effective Lifetime
- **Category:** Code Quality
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/Web/HealthChecks/ApiHealthCheck.cs

Same as CWE-772: the undisposed HttpClient in ApiHealthCheck.CheckHealthAsync() holds a socket/handle that is never released. Each health check invocation leaks a socket handle, which under high-frequency health check polling can exhaust available socket descriptors.

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs, docker-compose.yml

AuthorizationConstants.cs contains 'public const string DEFAULT_PASSWORD = "Pass@word1"' (with a TODO comment acknowledging this is not for production). This password is used as the seed default password for admin/demo users in AppIdentityDbContextSeed. Additionally, docker-compose.yml hard-codes 'SA_PASSWORD=@someThingComplicated1234' as the SQL Server SA account password.

### CWE-321: Use of Hard-coded Cryptographic Key
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs, src/Infrastructure/Identity/IdentityTokenClaimService.cs

AuthorizationConstants.JWT_SECRET_KEY is set to the literal string 'SecretKeyOfDoomThatMustBeAMinimumNumberOfBytes' (with a TODO comment: 'Change this to an environment variable'). IdentityTokenClaimService.GetTokenAsync() converts this constant to bytes via Encoding.ASCII.GetBytes(AuthorizationConstants.JWT_SECRET_KEY) and uses it as the HMAC-SHA256 signing key for all issued JWT tokens. A well-known, static signing key allows any party with knowledge of the source code to forge valid JWT tokens.

### CWE-778: Insufficient Logging
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 3
- **Files:** src/PublicApi/AuthEndpoints/AuthenticateEndpoint.cs

AuthenticateEndpoint.HandleAsync() processes login requests and sets response.IsLockedOut, response.IsNotAllowed, and response.RequiresTwoFactor flags but does not log any of these security-relevant failure conditions. Failed authentication attempts, account lockouts, and two-factor redirects occur silently with no audit trail in the application logs, making it impossible to detect brute-force or credential-stuffing attacks from logs alone.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs, src/ApplicationCore/Constants/AuthorizationConstants.cs, docker-compose.yml, src/Web/appsettings.Docker.json, src/PublicApi/appsettings.Docker.json

Multiple hard-coded credentials exist: (1) DEFAULT_PASSWORD='Pass@word1' and JWT_SECRET_KEY='SecretKeyOfDoomThatMustBeAMinimumNumberOfBytes' in AuthorizationConstants.cs; (2) SA_PASSWORD=@someThingComplicated1234 in docker-compose.yml environment section; (3) SQL Server connection strings with SA credentials (User Id=sa;****** committed in appsettings.Docker.json for both Web and PublicApi. All three constants are acknowledged with TODO comments in source but remain in the committed codebase.

### CWE-434: Unrestricted Upload of File with Dangerous Type
- **Category:** File & Path Security
- **Severity:** mandatory
- **Story Points:** 8
- **Files:** src/PublicApi/ImageValidators.cs, src/PublicApi/CatalogItemEndpoints/CreateCatalogItemEndpoint.cs, src/BlazorShared/Models/CatalogItem.cs

The CreateCatalogItemEndpoint accepts a Base64-encoded image string (pictureBase64) from the client and writes it to disk via UpdatePictureUri. ImageValidators.IsValidImage checks the file extension and size but does NOT validate actual file content (magic bytes/MIME sniffing). An attacker could submit a malicious file with a .jpg/.png extension that contains executable or script content, bypassing the extension-only check. The validation in src/PublicApi/ImageValidators.cs relies solely on Path.GetExtension(fileName) and a byte-length check, with no content-type or magic byte verification.
