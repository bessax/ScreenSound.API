# 🎵 ScreenSound API
 
API REST em **.NET 8** para gerenciar um catálogo de **artistas**, **músicas** e **gêneros musicais**, construída com **Minimal APIs**, **Entity Framework Core** e **SQL Server**.

---
 
## ✨ Funcionalidades
 
- CRUD de **Artistas** (nome, biografia e foto de perfil).
- CRUD de **Músicas**, com ano de lançamento e vínculo a um artista.
- Relação **muitos-para-muitos** entre músicas e gêneros: gêneros já existentes são reaproveitados pelo nome, e os novos são criados junto com a música.
- Cadastro, listagem, busca e exclusão de **Gêneros**.
- Busca por nome **sem diferenciar maiúsculas de minúsculas**.
- **DTOs com `record`** (`Request` e `Response`) separando a API das entidades.
- Serialização JSON com `ReferenceHandler.IgnoreCycles`, evitando ciclos entre entidades relacionadas.
- Documentação interativa com **Swagger**.
- Aplicativo de **console** original do ScreenSound, na mesma solução.
## 🧰 Tecnologias
 
| Tecnologia | Uso |
|---|---|
| C# e .NET 8 | Linguagem e plataforma |
| ASP.NET Core Minimal API | Endpoints da API |
| Entity Framework Core 7 + SQL Server | ORM e banco de dados |
| Lazy Loading Proxies | Carregamento tardio de relacionamentos |
| Swashbuckle (Swagger) | Documentação e testes da API |
 
## 🏗️ Estrutura da solução
 
```
ScreenSound.sln
├── ScreenSound.Shared.Modelos   # Entidades: Artista, Musica, Genero
├── ScreenSound.Shared.Dados     # DbContext, DAL<T> genérico e Migrations
├── ScreenSound.API              # Minimal API (endpoints, requests e responses)
└── ScreenSound                  # Aplicativo de console (menu interativo)
```
 
```
API ──▶ DAL<T> ──▶ EF Core ──▶ SQL Server
```
 
## ✅ Pré-requisitos
 
- [.NET SDK 8.0](https://dotnet.microsoft.com/download/dotnet/8.0)
- SQL Server ou **SQL Server LocalDB** (instalado junto com o Visual Studio, no Windows)
- Ferramenta do EF Core:
```bash
dotnet tool install --global dotnet-ef
```
 
## 🚀 Como executar
 
### 1. Clone o repositório
 
```bash
git clone https://github.com/<seu-usuario>/ScreenSound.API.git
cd ScreenSound.API
```
 
### 2. Configure a conexão com o banco
 
A API lê a string de conexão da chave `ConnectionStrings:ScreenSoundDB` em `ScreenSound.API/appsettings.json`. Para desenvolvimento local, use o **LocalDB**:
 
```json
"ConnectionStrings": {
  "ScreenSoundDB": "Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=ScreenSoundV0;Integrated Security=True;Encrypt=False"
}
```
 
> 🔐 **Nunca versione credenciais.** Para qualquer banco com usuário e senha (como o Azure SQL), guarde a string de conexão em **User Secrets** ou em variáveis de ambiente:
>
> ```bash
> cd ScreenSound.API
> dotnet user-secrets init
> dotnet user-secrets set "ConnectionStrings:ScreenSoundDB" "<sua-string-de-conexao>"
> ```
 
### 3. Crie o banco (Migrations)
 
```bash
dotnet ef database update --project ScreenSound.Shared.Dados --startup-project ScreenSound.API
```
 
### 4. Execute a API
 
```bash
dotnet run --project ScreenSound.API --launch-profile https
```
 
- Swagger: `https://localhost:7089/Swagger/index.html`
- HTTP: `http://localhost:5241/Swagger/index.html`
## 📡 Endpoints
 
| Método | Rota | Descrição |
|---|---|---|
| `GET` | `/Artistas` | Lista todos os artistas |
| `GET` | `/Artistas/{nome}` | Busca artista pelo nome |
| `POST` | `/Artistas` | Cadastra artista |
| `PUT` | `/Artistas` | Atualiza nome e biografia |
| `DELETE` | `/Artistas/{id}` | Remove artista |
| `GET` | `/Musicas` | Lista todas as músicas |
| `GET` | `/Musicas/{nome}` | Busca música pelo nome |
| `POST` | `/Musicas` | Cadastra música (com gêneros) |
| `PUT` | `/Musicas` | Atualiza nome e ano de lançamento |
| `DELETE` | `/Musicas/{id}` | Remove música |
| `GET` | `/Generos` | Lista gêneros |
| `GET` | `/Generos/{nome}` | Busca gênero pelo nome |
| `POST` | `/Generos` | Cadastra gênero |
| `DELETE` | `/Generos/{id}` | Remove gênero |
 
### Exemplos de requisição
 
**Cadastrar artista**
 
```http
POST /Artistas
Content-Type: application/json
 
{
  "nome": "Cazuza",
  "bio": "Cantor e compositor brasileiro."
}
```
 
**Cadastrar música com gêneros**
 
```http
POST /Musicas
Content-Type: application/json
 
{
  "nome": "Exagerado",
  "artistaId": 1,
  "anoLancamento": 1985,
  "generos": [
    { "nome": "Rock", "descricao": "Rock nacional" }
  ]
}
```
 
**Atualizar artista**
 
```http
PUT /Artistas
Content-Type: application/json
 
{
  "id": 1,
  "nome": "Cazuza",
  "bio": "Nova biografia."
}
```

## 📄 Licença
 
Defina aqui a licença do projeto (por exemplo, [MIT](https://opensource.org/licenses/MIT)).
 
---
 
Feito com 💙 durante os estudos de **.NET, Minimal APIs e Entity Framework Core**.
 
