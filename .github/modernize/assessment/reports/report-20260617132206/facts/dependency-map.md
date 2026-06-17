# Dependency Map

This dependency map summarizes declared NuGet dependencies for eShopOnWeb across its .NET solution, using centralized package management. The project declares approximately 36 non-test and 9 test-scoped package dependencies.

## Dependencies

```mermaid
flowchart LR
    AppD["eShopOnWeb"]
    CPMD["Directory.Packages.props central versions"]

    subgraph WebD["Web Frameworks"]
        AspMvcD["Microsoft.AspNetCore.Mvc v2.2.0"]
        AspCompD["Microsoft.AspNetCore.Components.WebAssembly v8.0.2"]
        MinimalApiD["MinimalApi.Endpoint v1.3.0"]
        ArdEndpointD["Ardalis.ApiEndpoints v4.1.0"]
    end

    subgraph DbD["Database ORM"]
        EfSqlD["EFCore.SqlServer v8.0.2"]
        EfMemD["EFCore.InMemory v8.0.2"]
        EfToolsD["EFCore.Tools v8.0.2"]
        ArdSpecEfD["Ardalis.Specification.EFCore v7.0.0"]
    end

    subgraph SecD["Security"]
        JwtBearerD["AspNetCore.JwtBearer v8.0.2"]
        IdentityEfD["AspNetCore.Identity.EFCore v8.0.2"]
        IdentityUiD["AspNetCore.Identity.UI v8.0.2"]
        JwtTokensD["System.IdentityModel.Tokens.Jwt v7.3.1"]
        ClaimsD["System.Security.Claims v4.3.0"]
    end

    subgraph ObsD["Observability"]
        SwaggerD["Swashbuckle.AspNetCore v6.5.0"]
        SwaggerUiD["Swashbuckle.SwaggerUI v6.5.0"]
        SwaggerAnnD["Swashbuckle.Annotations v6.5.0"]
    end

    subgraph UtilD["Utilities"]
        MediatrD["MediatR v12.0.1"]
        AutoMapD["AutoMapper.Extensions.Microsoft.DI v12.0.1"]
        GuardD["Ardalis.GuardClauses v4.0.1"]
        ResultD["Ardalis.Result v7.0.0"]
        SpecD["Ardalis.Specification v7.0.0"]
        JsonD["System.Text.Json v8.0.3"]
        FluentD["FluentValidation v11.9.0"]
    end

    subgraph CloudD["Cloud and Platform"]
        AzIdD["Azure.Identity v1.10.4"]
        AzKvD["Azure.Configuration.Secrets v1.3.1"]
        ContainerToolsD["VS Azure Containers Tools v1.19.6"]
        LibmanD["Web.LibraryManager.Build v2.1.175"]
    end

    AppD -->|"web"| WebD
    AppD -->|"persistence"| DbD
    AppD -->|"security"| SecD
    AppD -->|"api docs"| ObsD
    AppD -->|"application utilities"| UtilD
    AppD -->|"platform integration"| CloudD

    CPMD -.->|"manages versions"| AspCompD
    CPMD -.->|"manages versions"| EfSqlD
    CPMD -.->|"manages versions"| JwtBearerD
    CPMD -.->|"manages versions"| MediatrD
    CPMD -.->|"manages versions"| AzIdD
```

### Dependency Summary

| Category | Count | Key Libraries | Notes |
|---|---:|---|---|
| Web Frameworks | 4 | ASP.NET Core Components, MinimalApi.Endpoint | MVC and Blazor-based web/API surface |
| Database ORM | 4 | EF Core SqlServer, EF Core InMemory | SQL Server primary persistence with in-memory fallback/testing |
| Security | 5 | ASP.NET Core Identity, JwtBearer | Cookie and JWT based auth paths are both present |
| Observability | 3 | Swashbuckle packages | OpenAPI generation and Swagger UI for API inspection |
| Utilities | 7 | MediatR, AutoMapper, Ardalis packages | Core business logic and mapping support libraries |
| Cloud and Platform | 4 | Azure Identity, Key Vault config | Azure secret integration and local build tooling |

### Version & Compatibility Risks

The solution targets .NET 8 and generally uses 8.x framework packages, but several dependencies show modernization risk: `Microsoft.AspNetCore.Mvc` remains on legacy 2.2.0, `System.Text.Json` 8.0.3 is currently flagged by vulnerability advisories, and `Azure.Identity` 1.10.4 has published moderate-severity advisories. These versions should be reviewed during upgrade and security remediation planning.

### Notable Observations

- Central package management via `Directory.Packages.props` helps enforce consistent versions across all projects.
- Both cookie-based Identity UI and JWT bearer authentication libraries are included, indicating mixed auth mechanisms across web and API surfaces.
- EF Core SQL Server and InMemory providers are both declared broadly, supporting production and test or local fallback scenarios.
- Tooling packages such as LibraryManager and code generation dependencies can influence CI reliability even though they are not runtime dependencies.

## Test Dependencies

| Framework | Version | Notes |
|---|---|---|
| Microsoft.NET.Test.Sdk | 17.9.0 | Base test runner SDK across test projects |
| xUnit | 2.7.0 | Primary unit and integration test framework |
| xunit.runner.visualstudio | 2.5.6 | Visual Studio test adapter |
| xunit.runner.console | 2.7.0 | Console test runner support |
| MSTest.TestFramework | 3.2.2 | Used in PublicApiIntegrationTests |
| MSTest.TestAdapter | 3.2.2 | Runner integration for MSTest |
| NSubstitute | 5.1.0 | Mocking library for unit/integration tests |
| NSubstitute.Analyzers.CSharp | 1.0.17 | Analyzer support for NSubstitute usage |
| coverlet.collector | 6.0.2 | Code coverage data collection |

Total test-scope dependencies: 9

The repository has mature test infrastructure with both xUnit and MSTest in use. Mixed frameworks are workable but may increase long-term maintenance overhead for shared test tooling and conventions.
