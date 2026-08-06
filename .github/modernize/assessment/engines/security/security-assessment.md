# Security Assessment Report

**Generated:** 2026-08-06T11:26:42.0000000Z

## Summary

| Metric | Count |
|--------|-------|
| Total Findings | 3 |
| CVE Vulnerabilities | 0 |
| CWE Vulnerabilities | 3 |
| Total Rules Assessed | 59 |
| Rules Passed | 56 |

### By Severity

| Severity | Count |
|----------|-------|
| mandatory | 0 |
| optional | 2 |
| potential | 1 |

## CVE Findings (Dependency Vulnerabilities)

No CVE findings meeting the critical severity threshold.

## CWE Findings (Code-Level Vulnerabilities)

### CWE-259: Use of Hard-coded Password
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs:8, src/Infrastructure/Identity/AppIdentityDbContextSeed.cs:21

A hard-coded default password constant is used to create seeded users in AppIdentityDbContextSeed, exposing static credential material.

### CWE-321: Use of Hard-coded Cryptographic Key
- **Category:** Credentials & Secrets
- **Severity:** potential
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs:11, src/PublicApi/Program.cs:54

JWT signing key is hard-coded in AuthorizationConstants and consumed directly in PublicApi JWT configuration.

### CWE-798: Use of Hard-coded Credentials
- **Category:** Credentials & Secrets
- **Severity:** optional
- **Story Points:** 5
- **Files:** src/ApplicationCore/Constants/AuthorizationConstants.cs:5, src/ApplicationCore/Constants/AuthorizationConstants.cs:8, src/ApplicationCore/Constants/AuthorizationConstants.cs:11

Authentication-related constants include hard-coded auth key, password, and JWT secret key credentials in source.

