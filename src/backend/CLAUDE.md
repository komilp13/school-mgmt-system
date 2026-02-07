# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

School management system backend — a .NET 10 ASP.NET Core Web API. Currently in early stages with a single API project (`Simpl.School.Api`).

## Build & Run Commands

```bash
# Build
dotnet build Simpl.School.sln

# Run (http://localhost:5175, https://localhost:7194)
dotnet run --project Simpl.School.Api

# Run with hot reload
dotnet watch --project Simpl.School.Api

# Run tests (when test projects exist)
dotnet test Simpl.School.sln
```

## Architecture

Clean Architecture with four layers:

| Project | Role | References |
|---------|------|------------|
| `Simpl.School.Domain` | Entities, value objects, enums | (none — innermost) |
| `Simpl.School.Application` | Use cases, interfaces, DTOs | Domain |
| `Simpl.School.Infrastructure` | DB context, repos, external services | Application |
| `Simpl.School.Api` | Endpoints, middleware | Application + Infrastructure |

- **Solution**: `Simpl.School.sln` — single solution file at repo root
- **API Project**: `Simpl.School.Api/` — ASP.NET Core minimal API (.NET 10)
- **Entry Point**: `Program.cs` uses top-level statements with minimal API pattern (no Startup class, no controllers)
- **DI Registration**: Each layer exposes an `Add*Services()` extension method in `DependencyInjection.cs`, called from `Program.cs`
- **OpenAPI**: Enabled via `Microsoft.AspNetCore.OpenApi`, mapped at `/openapi/v1.json` in Development
- **Nullable reference types** and **implicit usings** are enabled
