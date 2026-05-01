# 🚀 Projeto Alura - Full Stack Development

Estrutura monorepo com **Front-end Angular** e **Back-end .NET Core**.

## 📦 Estrutura do Projeto

```
Alura/
├── client/                    # 🎨 Aplicação Angular (Front-end)
│   ├── src/
│   ├── angular.json
│   ├── package.json
│   ├── tsconfig.json
│   └── ...
│
├── server/                    # 🔧 API .NET Core (Back-end)
│   ├── AluraAPI/              # Projeto WebAPI
│   │   ├── Controllers/
│   │   ├── Program.cs
│   │   ├── AluraAPI.csproj
│   │   └── ...
│   └── ...
│
├── .vscode/
│   ├── alura.code-workspace   # 📝 Multi-root workspace (ABRA ESTE ARQUIVO!)
│   └── ...
│
├── WORKSPACE_ORGANIZATION.md  # 📖 Guia detalhado de organização
└── README.md                  # Este arquivo
```

---

## 🎯 Como Começar

### 1️⃣ Abrir o Workspace no VS Code

Abra o VS Code e carregue o workspace com:

```
File → Open Workspace from File... 
→ Selecione: C:\Users\alves\Projetos\Alura\.vscode\alura.code-workspace
```

Ou use a linha de comando:

```powershell
cd C:\Users\alves\Projetos\Alura
code .\.vscode\alura.code-workspace
```

### 2️⃣ Instalar Dependências

#### Front-end (Angular)
```powershell
cd client
npm install
```

#### Back-end (.NET Core)
```powershell
cd server\AluraAPI
dotnet restore
```

### 3️⃣ Rodar os Serviços

Você pode usar os **tasks predefinidos** no workspace:

**Via VS Code:**
- Pressione `Ctrl+Shift+B` → Escolha a tarefa desejada
- Ou vá para `Terminal → Run Task...`

**Via Terminal:**

```powershell
# Terminal 1 - Front-end
cd client
npm start          # Será executado em http://localhost:4200

# Terminal 2 - Back-end  
cd server\AluraAPI
dotnet run        # Será executado em https://localhost:5001
```

---

## 🛠 Tarefas Disponíveis

| Tarefa | Descrição | Comando |
|--------|-----------|---------|
| ▶️ Run Frontend | Inicia o servidor Angular | `npm start` |
| ▶️ Run Backend | Inicia a API .NET | `dotnet run` |
| 🔨 Build Frontend | Compila Angular | `npm run build` |
| 🔨 Build Backend | Compila .NET | `dotnet build` |
| ✅ Test Frontend | Testa Angular | `npm test` |
| ✅ Test Backend | Testa .NET | `dotnet test` |

---

## 🔌 Portas Padrão

| Serviço | Porta | URL |
|---------|-------|-----|
| Angular Dev Server | 4200 | http://localhost:4200 |
| .NET Core HTTP | 5000 | http://localhost:5000 |
| .NET Core HTTPS | 5001 | https://localhost:5001 |

---

## 📚 Documentação

- **[WORKSPACE_ORGANIZATION.md](./WORKSPACE_ORGANIZATION.md)** - Guia completo de organização e melhores práticas
- **[client/README.md](./client/README.md)** - Documentação específica do Angular
- **[server/AluraAPI/README.md](./server/AluraAPI/README.md)** - Documentação específica do .NET Core

---

## 🔗 Comunicação Front-end & Back-end

### Configurar CORS no Backend

O CORS já está configurado em `server/AluraAPI/Program.cs` para aceitar requisições do Angular.

### Fazer Requisições HTTP do Angular

Exemplo de serviço:

```typescript
// src/app/services/api.service.ts
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ApiService {
  private apiUrl = 'http://localhost:5000/api';

  constructor(private http: HttpClient) {}

  getData() {
    return this.http.get(`${this.apiUrl}/data`);
  }
}
```

---

## 🧪 Testes

### Angular
```powershell
cd client
npm test          # Executa testes com Vitest
```

### .NET Core
```powershell
cd server\AluraAPI
dotnet test
```

---

## 📝 Git & Versionamento

```
main (produção)
  └── develop (desenvolvimento)
        ├── feature/frontend/...
        ├── feature/backend/...
        └── feature/shared/...
```

**Commit Convention:**
```
[Frontend] Descrição da mudança
[Backend] Descrição da mudança
[Shared] Descrição compartilhada
```

---

## 🔒 Variáveis de Ambiente

Crie arquivos `.env` em ambos os projetos:

**client/.env**
```
NG_APP_API_URL=http://localhost:5000/api
```

**server/AluraAPI/.env** (ou appsettings.json)
```
ConnectionString=Server=localhost;Database=AluraDB;...
```

---

## 🚀 Deploy

### Frontend (Build Production)
```powershell
cd client
npm run build
# Resultado em: client/dist/
```

### Backend (Publish)
```powershell
cd server\AluraAPI
dotnet publish -c Release -o ./publish
```

---

## 📋 Checklist de Inicialização

- [ ] Abrir workspace `alura.code-workspace` no VS Code
- [ ] Instalar dependências: `npm install` e `dotnet restore`
- [ ] Rodar ambos os serviços
- [ ] Testar acesso: http://localhost:4200 e http://localhost:5000
- [ ] Configurar CORS (já feito)
- [ ] Criar primeiro serviço HTTP no Angular
- [ ] Testar chamada API

---

## 💡 Dicas Úteis

1. **Multi-cursor editing:** Selecione múltiplas ocorrências com `Ctrl+D`
2. **Command Palette:** `Ctrl+Shift+P` para acessar todos os comandos
3. **Debug .NET:** Pressione `F5` para iniciar o debugger
4. **Debug Angular:** Use as DevTools do navegador ou VS Code Debugger
5. **Terminal integrado:** `Ctrl+`` ` para abrir/fechar terminal

---

## ❓ FAQ

**P: Como adiciono uma nova biblioteca Angular?**
```powershell
cd client
ng add @angular/material
```

**P: Como adiciono um novo pacote NuGet?**
```powershell
cd server\AluraAPI
dotnet add package NomePacote
```

**P: Por que não consigo conectar o frontend com o backend?**
- Verifique se CORS está configurado
- Verifique se ambos os serviços estão rodando
- Verifique as portas corretas no código

---

## 👥 Contribuindo

1. Crie uma branch a partir de `develop`
2. Faça suas alterações
3. Abra um Pull Request
4. Aguarde revisão

---

**Última atualização:** Maio 2026  
**Versão:** 1.0.0

