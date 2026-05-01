# Organização de Workspaces - Projeto Alura

## 📁 Estrutura do Projeto

```
Alura/
├── client/                 # Front-end Angular
│   ├── src/
│   ├── package.json
│   ├── angular.json
│   └── ...
├── server/                 # Back-end .NET Core
│   ├── AluraAPI/
│   ├── AluraAPI.sln
│   └── ...
├── WORKSPACE_ORGANIZATION.md
└── README.md
```

## 🎯 Estratégias de Workspace Recomendadas

### Opção 1: **Multi-Root Workspace (Recomendado)**

A melhor forma de trabalhar com ambos os projetos simultaneamente no VS Code.

**Como configurar:**

1. Crie uma pasta `.vscode` na raiz do projeto (`C:\Users\alves\Projetos\Alura`)

2. Dentro de `.vscode`, crie um arquivo `alura.code-workspace`:

```json
{
  "folders": [
    {
      "path": "client",
      "name": "🎨 Frontend (Angular)"
    },
    {
      "path": "server",
      "name": "🔧 Backend (.NET Core)"
    }
  ],
  "settings": {
    "python.defaultInterpreterPath": "${workspaceFolder}/.venv/bin/python",
    "files.exclude": {
      "**/.git": true,
      "**/node_modules": true,
      "**/bin": true,
      "**/obj": true
    }
  },
  "extensions": {
    "recommendations": [
      "Angular.ng-template",
      "ms-dotnettools.csharp",
      "ms-dotnettools.vscode-dotnet-runtime",
      "eamodio.gitlens",
      "esbenp.prettier-vscode",
      "ms-vscode.makefile-tools"
    ]
  }
}
```

3. Abra o VS Code e vá para `File > Open Workspace from File...`

4. Selecione o arquivo `alura.code-workspace`

**Vantagens:**
- ✅ Visualiza ambos os projetos simultaneamente
- ✅ Melhor contexto entre front-end e back-end
- ✅ Configurações compartilhadas de workspace
- ✅ Terminal único para rodar ambos os serviços
- ✅ Facilita debugging paralelo

### Opção 2: Workspaces Separados

Use instâncias separadas do VS Code para cada projeto.

**Cliente:**
```powershell
cd C:\Users\alves\Projetos\Alura\client
code .
```

**Servidor:**
```powershell
cd C:\Users\alves\Projetos\Alura\server\AluraAPI
code .
```

**Vantagens:**
- ✅ Menos confusão visual
- ✅ Melhor para equipes separadas

**Desvantagens:**
- ❌ Difícil coordenar mudanças entre os projetos
- ❌ Precisa de múltiplas instâncias do VS Code

### Opção 3: Monorepo (Alternativo)

Se preferir centralizar tudo:

```
Alura/
├── apps/
│   ├── client/
│   └── server/
├── shared/           # Código compartilhado (DTOs, interfaces)
├── docs/
└── package.json      # Workspace raiz (se usar npm workspaces)
```

---

## 🚀 Como Usar Este Projeto

### Terminal 1 - Front-end
```powershell
cd C:\Users\alves\Projetos\Alura\client
npm start
# Acessar: http://localhost:4200
```

### Terminal 2 - Back-end
```powershell
cd C:\Users\alves\Projetos\Alura\server\AluraAPI
dotnet run
# Acessar: https://localhost:5001 ou http://localhost:5000
```

---

## 📋 Extensões Recomendadas para VS Code

**Para Angular:**
- Angular Language Service
- Angular Schematics
- Material Icon Theme

**Para .NET Core:**
- C# (Official)
- C# Extensions
- NuGet Package Manager
- REST Client (para testar APIs)

**Para Ambos:**
- Git Graph
- GitLens
- Prettier
- ESLint
- Postman (ou Thunder Client)

---

## 🔌 CORS e Comunicação

### Configurar CORS no .NET Core

No arquivo `Program.cs`:

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("http://localhost:4200")
              .AllowAnyMethod()
              .AllowAnyHeader();
    });
});

app.UseCors("AllowAngularClient");
```

### Chamadas HTTP do Angular

No seu serviço Angular:

```typescript
import { HttpClient } from '@angular/common/http';

export class ApiService {
  private apiUrl = 'http://localhost:5000/api';

  constructor(private http: HttpClient) {}

  getData() {
    return this.http.get(`${this.apiUrl}/data`);
  }
}
```

---

## 💾 Controle de Versão (Git)

Estrutura recomendada para o `.gitignore` na raiz:

```
# Node
client/node_modules/
client/dist/
client/.angular/

# .NET
server/**/bin/
server/**/obj/
server/**/*.user
server/**/.vs/
server/**/.vscode/

# Variáveis de ambiente
.env
.env.local

# IDE
.vscode/
*.code-workspace
```

---

## 📊 Estrutura de Branches Recomendada

```
main                    # Produção
├── develop            # Desenvolvimento
│   ├── feature/frontend/...
│   ├── feature/backend/...
│   └── feature/shared/...
└── hotfix/...
```

---

## 🎓 Resumo das Melhores Práticas

| Aspecto | Recomendação |
|---------|-------------|
| **Organização** | Monorepo com pastas `client` e `server` |
| **Workspace** | Multi-Root Workspace (.code-workspace) |
| **Portas** | Front (4200), Back (5000/5001) |
| **Comunicação** | CORS configurado no backend |
| **Ambiente** | Arquivo `.env` para variáveis |
| **Versionamento** | Git com branches separadas por feature |
| **Testes** | Jest (Angular), xUnit (.NET Core) |

---

## 📚 Links Úteis

- [Angular CLI Workspace](https://angular.io/guide/workspace-config)
- [VS Code Multi-root Workspaces](https://code.visualstudio.com/docs/editor/multi-root-workspaces)
- [ASP.NET Core CORS Documentation](https://docs.microsoft.com/pt-br/aspnet/core/security/cors)
- [npm Workspaces](https://docs.npmjs.com/cli/v7/using-npm/workspaces)

---

**Criado em:** Maio 2026  
**Para:** Projeto Alura - Full Stack Development
