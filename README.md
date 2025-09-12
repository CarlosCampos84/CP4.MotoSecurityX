# 🚀 CP4 - 2TDS — Clean Code, DDD e Clean Architecture com .NET 8

API do Checkpoint 4, baseada no domínio **Mottu/MotoSecurity**. O projeto aplica **DDD**, **Clean Architecture** e boas práticas de **Clean Code**.

## 👥 Integrantes do Grupo
- **Caio Henrique** – RM: 554600  
- **Carlos Eduardo** – RM: 555223  
- **Antônio Lino** – RM: 554518

---

## 🎯 Objetivo
API Web para controle e monitoramento de motos e pátios (**MotoSecurityX**) usando .NET 8, EF Core, Sqlite e Swagger, com organização em camadas e modelagem orientada a domínio.

---

## 🧭 Arquitetura (Camadas)

CP4.MotoSecurityX.Api/ -> Controllers, Program, Swagger, appsettings
CP4.MotoSecurityX.Application/ -> Use cases (Handlers), DTOs
CP4.MotoSecurityX.Domain/ -> Entidades, Value Objects, Interfaces (Repos)
CP4.MotoSecurityX.Infrastructure/ -> EF Core (DbContext, Migrations), Repos EF, DI

yaml
Copiar código

**Princípios aplicados**
- **Inversão de Dependência**: interfaces de repositório no Domain; implementações na Infrastructure.
- **Baixo acoplamento** entre camadas; a API não referencia EF diretamente.
- **Regras de negócio** concentradas no domínio/use cases.

---

## 🧩 Modelagem de Domínio (DDD)
- **Entidades**: `Moto`, `Patio`  
- **Agregado Raiz**: `Patio` (relação 1-N com `Motos`)  
- **Value Object**: `Placa` (mapeada como *owned type*; índice único na tabela de `Motos`)  
- **Regras (exemplos)**:
  - Placa única
  - Moto pode estar associada a um pátio (FK opcional)
  - (Recomendado) Métodos de comportamento como `Patio.AdicionarMoto(...)`, `Moto.EntrarNoPatio(...)`

---

## 🔧 Requisitos
- .NET 8 SDK
- (Opcional) `dotnet-ef` como **ferramenta local** (manifesto já em `.config/dotnet-tools.json`)

---

## ▶️ Como executar localmente

Na **raiz** do repositório:

```powershell
# Restaurar e compilar
dotnet restore
dotnet build

# (uma vez) restaurar a ferramenta local dotnet-ef
dotnet tool restore

# Criar/atualizar o banco Sqlite (se ainda não existir)
dotnet ef database update `
  -p .\CP4.MotoSecurityX.Infrastructure\ `
  -s .\CP4.MotoSecurityX.Api\

# Subir a API
dotnet run --project .\CP4.MotoSecurityX.Api\
Swagger: a URL exata aparece no terminal (ex.: http://localhost:5102/swagger).

Banco: Sqlite (Data Source=motosecurityx.db no appsettings.json da API).
```

## 🌐 Endpoints (exemplos)
Criar Pátio
`POST` /api/patios

```json
{
  "nome": "Pátio Central",
  "endereco": "Rua 1"
}
```

Criar Moto
`POST` /api/motos

```json
{
  "placa": "ABC1D23",
  "modelo": "Mottu 110i"
}
```

Listar Motos
`GET` /api/motos

Obter Moto por Id
`GET` /api/motos/{id}

Mover Moto para um Pátio
`POST` /api/motos/{id}/mover

```json
{ "patioId": "<GUID do pátio>" }
```
Todos os endpoints podem ser exercitados via Swagger.

## 🗃️ Persistência & Migrations
EF Core 8 + Sqlite.

Migration InitialCreate versionada em Infrastructure/Data/Migrations.

appsettings.json na API contém a connection string.

## 🧼 Clean Code
SRP/DRY/KISS/YAGNI

Controllers finos, uso de DTOs e handlers

Nomes claros e métodos pequenos

## 📝 Convenção de Commits
feat: nova funcionalidade

fix: correção

chore: manutenção (ex.: .gitignore, tool manifest, migrations)

refactor:, docs:, etc.

Exemplos reais do repo:

feat(api+infra): DI + Swagger + controllers; ajustes de mapeamento

chore(repo): ignorar SQLite no .gitignore + adicionar manifest da tool e migrations

## 🌟 Propósito
“Código limpo sempre parece que foi escrito por alguém que se importa.” — Uncle Bob
