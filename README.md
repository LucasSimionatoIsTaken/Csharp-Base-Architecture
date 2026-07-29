# C# Base Architecture

Este projeto é uma base para APIs em .NET, estruturado com arquitetura de camadas. Seu objetivo é acelerar o desenvolvimento de novos projetos, promover boas práticas e servir como referência técnica.

[![Docker](https://img.shields.io/badge/Docker-Container-blue?logo=docker)](https://www.docker.com/)
[![.NET](https://img.shields.io/badge/.NET-9.0-blue?logo=dotnet)](https://dotnet.microsoft.com/)
[![SQL Server](https://img.shields.io/badge/Database-SQL_Server-4479A1?logo=microsoft-sql-server)](https://www.microsoft.com/sql-server)

## Sumário

- [Por que usar este Boilerplate?](#-por-que-usar-este-boilerplate)
- [Funcionalidades Inclusas](#-funcionalidades-inclusas)
- [Estrutura do Projeto](#-estrutura-do-projeto)
- [Começando](#-começando)
- [Como Usar](#-como-usar)
- [Customização do Banco de Dados](#-customização-do-banco-de-dados)
- [Próximos Passos](#-próximos-passos)
- [Feedback](#-feedback)

## 🎯 Por que usar este Boilerplate?

- **Acelere o desenvolvimento:** comece com uma estrutura robusta e pré-configurada, economizando tempo de setup.
- **Foco nas regras de negócio:** concentre-se nas regras da aplicação em vez da infraestrutura repetitiva.
- **Fácil de manter:** a separação em camadas facilita manutenção e evolução sem aumentar desnecessariamente a complexidade.

## 📌 Funcionalidades Inclusas

- **Soft delete:** registros são marcados com `DeletedAt` em vez de removidos definitivamente.
- **Timestamps automáticos:** entidades compartilham `CreatedAt` e `UpdatedAt`, gerenciados automaticamente.
- **Repositório genérico:** abstração CRUD com suporte a filtros, includes e paginação.
- **Unit of Work:** coordena as operações de persistência.
- **Validação de requisições:** regras de DTOs de entrada com FluentValidation.
- **Respostas padronizadas:** a API retorna modelos consistentes para facilitar o consumo no frontend.
- **Autenticação JWT:** autenticação e autorização por bearer token já configuradas.
- **Swagger:** documentação interativa dos endpoints em ambiente de desenvolvimento.

## 📁 Estrutura do Projeto

O projeto segue uma arquitetura em camadas inspirada em DDD, com separação clara de responsabilidades.

```text
src/
  API/             # Controllers, filtros, configuração e inicialização da API
  Application/     # Casos de uso, validação, mapeamentos e respostas
  Core/            # Entidades, enums e regras centrais do domínio
  Infrastructure/  # EF Core, repositórios, migrations e opções
tests/
  IntegrationTests/# Testes de API e infraestrutura de testes
  UnitTests/       # Testes unitários
docker-compose.yml # API e SQL Server em containers
```

## 🚀 Começando

### Pré-requisitos

- [.NET 9 SDK](https://dotnet.microsoft.com/en-us/download)
- [Docker](https://www.docker.com/) para executar com containers
- SQL Server para execução local sem Docker

### Clone e configuração local

```bash
git clone https://github.com/LucasSimionatoIsTaken/csharp-ddd-boilerplate.git
cd csharp-ddd-boilerplate
```

Para execução local, atualize a connection string `Default` em `src/API/appsettings.json` para apontar para sua instância do SQL Server:

```json
"ConnectionStrings": {
  "Default": "Data Source=SEU_SERVIDOR;Initial Catalog=NOME_DO_BANCO;TrustServerCertificate=true;Integrated Security=true;"
}
```

### Execute o projeto

Com Docker, o Compose cria a API e o SQL Server:

```bash
docker compose up --build
```

Localmente, restaure as dependências e inicie a API. As migrations pendentes são aplicadas na inicialização:

```bash
dotnet restore BackBase.sln
dotnet run --project src/API/API.csproj
```

## 📚 Como Usar

Em ambiente de desenvolvimento, a documentação Swagger fica disponível na raiz da aplicação. Com Docker, acesse `http://localhost:5000/`; pela configuração local, acesse `https://localhost:5000/`.

Endpoints de exemplo:

- `POST /api/auth`: Faz login.
- `GET /api/users`: Listagem paginada dos usuários, requer JWT.
- `POST /api/users`: Cria um usuário.
- `PUT /api/users/{id}`: Atualiza um usuário.
- `DELETE /api/users/{id}`: Deleta um usuário (o registro não é apagado).

## Testes

Execute todos os testes com:

```bash
dotnet test BackBase.sln
```

Os testes de integração usam a configuração própria em `tests/IntegrationTests` e não exigem uma instância local do SQL Server.

## 🔧 Customização do Banco de Dados

Para usar outro SGBD, instale o provider do Entity Framework Core desejado, remova o provider do SQL Server e atualize o método `AddDbContext` em `src/API/Extensions/IServiceCollectionExtension.cs`. Por exemplo, para MySQL:

```bash
dotnet add src/Infrastructure/Infrastructure.csproj package Pomelo.EntityFrameworkCore.MySql
dotnet remove src/Infrastructure/Infrastructure.csproj package Microsoft.EntityFrameworkCore.SqlServer
```

Em seguida, substitua `UseSqlServer` pelo método correspondente do novo provider, como `UseMySql`.

Por último, remova a pasta `src/Infrastructure/Migrations` com as migrações anteriores (se houver) e adicione novamente com o comando

```bash
dotnet ef migrations add MigrationName --project src/Infrastructure --startup-project src/API
```

## 📚 Próximos Passos

- [x] Melhorar a documentação da API com Swagger
- [x] Melhorar a paginação com ordenação
- [x] Adicionar testes de integração
- [ ] Adicionar refresh token e recuperação de senha
- [ ] Adicionar seed de dados
- [ ] Adicionar serviço de e-mail e upload de arquivos
- [ ] Expandir os testes unitários

## 📝 Feedback

Este é um projeto pessoal, mas feedbacks são bem-vindos. Se você encontrar um bug ou tiver uma sugestão, sinta-se à vontade para abrir uma [Issue](https://github.com/LucasSimionatoIsTaken/csharp-ddd-boilerplate/issues).