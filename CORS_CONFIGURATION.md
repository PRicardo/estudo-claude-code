# 🔐 Configuração CORS para Comunicação Front-end & Back-end

Esta guia mostra como configurar CORS no seu backend .NET Core para aceitar requisições do Angular.

## 📍 Arquivo: `server/AluraAPI/Program.cs`

```csharp
using Microsoft.AspNetCore.Cors;

var builder = WebApplicationBuilder.CreateBuilder(args);

// ========== CORS Configuration ==========
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("http://localhost:4200")     // URL do Angular dev server
              .AllowAnyMethod()                          // GET, POST, PUT, DELETE, etc.
              .AllowAnyHeader()                          // Aceita qualquer header
              .AllowCredentials();                       // Permite cookies e autenticação
    });
});

// Add services to the container.
builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

var app = builder.Build();

// ========== Middleware ==========
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

// Aplicar CORS ANTES de UseAuthorization
app.UseCors("AllowAngularClient");

app.UseAuthorization();

app.MapControllers();

app.Run();
```

## 🌍 Configuração por Ambiente

### Desenvolvimento (localhost)
```csharp
if (app.Environment.IsDevelopment())
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("http://localhost:4200")
              .AllowAnyMethod()
              .AllowAnyHeader()
              .AllowCredentials();
    });
}
```

### Staging (pré-produção)
```csharp
if (app.Environment.IsStaging())
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("https://staging.alura.com")
              .AllowAnyMethod()
              .AllowAnyHeader()
              .AllowCredentials();
    });
}
```

### Produção
```csharp
if (app.Environment.IsProduction())
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("https://alura.com", "https://www.alura.com")
              .AllowAnyMethod()
              .AllowAnyHeader()
              .AllowCredentials();
    });
}
```

## 🔧 Configuração Alternativa (Mais Restritiva)

```csharp
options.AddPolicy("AllowAngularClient", policy =>
{
    policy.WithOrigins("http://localhost:4200")
          .WithMethods("GET", "POST", "PUT", "DELETE")      // Métodos específicos
          .WithHeaders("Content-Type", "Authorization")    // Headers específicos
          .WithExposedHeaders("X-Total-Count");             // Headers que o client pode ler
});
```

## ⚠️ NÃO RECOMENDADO - Permitir Tudo (Apenas para Testes)

```csharp
// ❌ NUNCA em produção!
options.AddPolicy("AllowAll", policy =>
{
    policy.AllowAnyOrigin()
          .AllowAnyMethod()
          .AllowAnyHeader();
});

app.UseCors("AllowAll");
```

## 📡 Testando CORS com cURL

```bash
# GET request
curl -H "Origin: http://localhost:4200" \
     -H "Access-Control-Request-Method: POST" \
     -H "Access-Control-Request-Headers: Content-Type" \
     -X OPTIONS \
     http://localhost:5000/api/dados

# POST request
curl -H "Origin: http://localhost:4200" \
     -H "Content-Type: application/json" \
     -X POST \
     -d '{"name":"Test"}' \
     http://localhost:5000/api/dados
```

## 🔌 Testando CORS com REST Client (VS Code)

Crie um arquivo `test.http`:

```http
### Test GET request
GET http://localhost:5000/api/dados HTTP/1.1
Host: localhost:5000
Origin: http://localhost:4200

### Test POST request
POST http://localhost:5000/api/dados HTTP/1.1
Host: localhost:5000
Origin: http://localhost:4200
Content-Type: application/json

{
  "name": "Novo Item"
}
```

Pressione "Send Request" acima de cada bloco.

## 🐛 Resolvendo Problemas de CORS

### Erro: "Access to XMLHttpRequest blocked by CORS policy"

**Solução 1:** Verifique se o CORS está configurado
```csharp
app.UseCors("AllowAngularClient");  // Deve estar APÓS UseRouting() mas ANTES de UseAuthorization()
```

**Solução 2:** Verifique a URL de origem
```typescript
// Angular deve fazer requisições para a URL correta
this.http.get('http://localhost:5000/api/dados')  // ✅ Correto
this.http.get('https://localhost:5000/api/dados') // ❌ Pode ser problema
```

**Solução 3:** Verifique os headers
```csharp
// Certifique-se de que os headers necessários estão permitidos
policy.WithHeaders("Content-Type", "Authorization", "X-Requested-With")
```

## 📊 Exemplo Completo com Autenticação

```csharp
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowAngularClient", policy =>
    {
        policy.WithOrigins("http://localhost:4200")
              .AllowAnyMethod()
              .AllowAnyHeader()
              .AllowCredentials()  // Importante para autenticação
              .WithExposedHeaders("Authorization", "Content-Type");
    });
});

// JWT Authentication
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(options => { /*...*/ });

var app = builder.Build();

app.UseCors("AllowAngularClient");
app.UseAuthentication();
app.UseAuthorization();

app.MapControllers();
app.Run();
```

## 🎯 Checklist CORS

- [ ] CORS está configurado em `Program.cs`
- [ ] `app.UseCors()` está após `app.UseRouting()` mas antes de `app.UseAuthorization()`
- [ ] Origin do Angular (`http://localhost:4200`) está na lista de origens permitidas
- [ ] Métodos HTTP necessários estão permitidos
- [ ] Headers necessários estão na lista permitida
- [ ] Você testou com cURL ou REST Client
- [ ] O backend está rodando em `http://localhost:5000`
- [ ] O frontend está rodando em `http://localhost:4200`

---

**Referências:**
- [Microsoft Docs - CORS](https://docs.microsoft.com/pt-br/aspnet/core/security/cors)
- [MDN - CORS](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/CORS)
- [Angular HttpClient](https://angular.io/guide/http)

