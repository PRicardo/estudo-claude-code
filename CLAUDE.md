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

#### Atomic Design

Components are organized under `src/app/components/` following the Atomic Design hierarchy:

```
src/app/components/
├── atoms/       # Basic HTML elements (button, input, label, icon)
├── molecules/   # Simple groups of atoms (search field, card, form field)
├── organisms/   # Complex sections composed of molecules (header, form, list)
├── templates/   # Page layouts without real data
└── pages/       # Templates with real data, connected to routes
```

Rules:
- A component may only import from levels below its own (e.g. a molecule imports atoms, never another molecule or higher)
- Every component must have a `.spec.ts` test file covering its essential use (inputs, outputs, and key rendered content)
- Name files as `<name>.component.ts` and tests as `<name>.component.spec.ts`, colocated in the same folder

### .NET Backend

- **Minimal API** style — all configuration lives in `Program.cs`
- **OpenAPI/Swagger** enabled in development via `Microsoft.AspNetCore.OpenApi`
- **CORS** policy `"AllowAngularClient"` must be registered in `Program.cs` to allow `http://localhost:4200` (see `CORS_CONFIGURATION.md` for the full setup)
- `appsettings.Development.json` is gitignored — local secrets and dev overrides go there

#### REST Principles

All endpoints must follow these rules:

- **Resource-based URLs** — nouns, never verbs (`/products`, `/orders/{id}`, not `/getProduct`)
- **Correct HTTP verbs** — `GET` (read, idempotent), `POST` (create), `PUT` (full replace), `PATCH` (partial update), `DELETE` (remove)
- **Proper status codes** — `200 OK`, `201 Created` (with `Location` header), `204 No Content`, `400 Bad Request`, `404 Not Found`, `409 Conflict`, `422 Unprocessable Entity`
- **Stateless** — no session state on the server; all context must come from the request
- **Consistent error body** — errors return `{ "type", "title", "status", "detail" }` (RFC 7807 Problem Details, built-in via `TypedResults.Problem`)
- **Plural resource names** — `/products` not `/product`
- **Nested resources only when ownership is strict** — `/orders/{id}/items` is valid; avoid nesting beyond two levels

### Ports

| Service | URL |
|---|---|
| Angular dev server | http://localhost:4200 |
| .NET API (HTTP) | http://localhost:5121 |
| .NET API (HTTPS) | https://localhost:7266 |

## Git Conventions

Branch naming: `feature/frontend/<name>`, `feature/backend/<name>`, `feature/shared/<name>`

Commit prefix: `[Frontend]`, `[Backend]`, or `[Shared]` before the message.
