# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Workspace Structure

Monorepo with two independent apps sharing a single git repository:

- `client/` — Angular 21.2 frontend with SSR (Server-Side Rendering via Express)
- `server/` — ASP.NET Core 10 minimal API backend

## Development Commands

### Frontend (run from `client/`)

```bash
npm start          # dev server at http://localhost:4200
npm run build      # production build → dist/
npm run watch      # build in watch mode (development)
npm test           # unit tests with Vitest
npm run serve:ssr:client  # run SSR build (after npm run build)
```

### Backend (run from `server/`)

```bash
dotnet run         # API at http://localhost:5121 / https://localhost:7266
dotnet build       # compile
dotnet test        # unit tests
dotnet restore     # restore NuGet packages
dotnet publish -c Release -o ./publish  # production build
```

## Architecture

### Angular Frontend

- **Standalone components** — no NgModules; all components use `standalone: true`
- **SSR enabled** — `src/server.ts` is the Express entry point; `src/main.server.ts` bootstraps the app server-side
- **Testing** — Vitest 4.x with jsdom (not Karma/Jasmine)
- **Strict TypeScript** — `strict: true`, `noImplicitOverride`, `noFallthroughCasesInSwitch`
- Routes are defined in `src/app/app.routes.ts`; app config (providers, hydration) in `src/app/app.config.ts`

### .NET Backend

- **Minimal API** style — all configuration lives in `Program.cs`
- **OpenAPI/Swagger** enabled in development via `Microsoft.AspNetCore.OpenApi`
- **CORS** policy `"AllowAngularClient"` must be registered in `Program.cs` to allow `http://localhost:4200` (see `CORS_CONFIGURATION.md` for the full setup)
- `appsettings.Development.json` is gitignored — local secrets and dev overrides go there

### Ports

| Service | URL |
|---|---|
| Angular dev server | http://localhost:4200 |
| .NET API (HTTP) | http://localhost:5121 |
| .NET API (HTTPS) | https://localhost:7266 |

## Git Conventions

Branch naming: `feature/frontend/<name>`, `feature/backend/<name>`, `feature/shared/<name>`

Commit prefix: `[Frontend]`, `[Backend]`, or `[Shared]` before the message.
