## Docker-Compose

No passo anterior, vimos como podemos construir uma imagem docker com vários estágios, aumentando a resiliência, reduzindo o tamanho e de forma mais segura do que com apenas um estágio. Agora vamos ver como podemos rodar mais aplicações além da nossa.

Utilizar o Docker Compose permite definir e executar múltiplos containers Docker como um único serviço, facilitando a gestão de aplicações complexas que dependem de várias partes, como uma Web API .NET Core e um banco de dados SQL Server. Este método é especialmente útil para garantir a consistência entre os ambientes de desenvolvimento, teste e produção, permitindo que toda a infraestrutura necessária para uma aplicação seja definida em um arquivo YAML de fácil compreensão e manipulação.

**Objetivo**: Configurar e executar uma aplicação .NET Core Web API juntamente com um SQL Server em containers, utilizando Docker Compose para manejar a comunicação entre eles.

---

Primeiro, vamos criar um arquivo chamado `docker-compose.yaml` na pasta da nossa aplicação, como o exemplo a seguir:

```yaml

version: '3.3'
services:
  webapi:
    container_name: aplicacao
    image: <ALTERE PARA O NOME DA IMAGEM CRIADA>
    ports:
      - "8080:8080"
    depends_on:
      - db
    environment:
      - ConnectionStrings__DefaultConnection=Server=database;Database=master;User=sa;Password=<ALTERE A SENHA>;Encrypt=false;

  db:
    container_name: database
    image: "mcr.microsoft.com/mssql/server:2022-latest"
    environment:
      SA_PASSWORD: "<ALTERE A SENHA>"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"

```{{copy}}

E claro, será necessário adicionar um código fonte para buscar as informações do banco. Para isso, vamos criar um arquivo na pasta `Controllers` chamado `LivrosController.cs`. Para simplificar (mesmo não sendo uma boa prática) vamos colocar grande parte do código neste arquivo:

```cs
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using System;
using System.Collections.Generic;
using System.ComponentModel.DataAnnotations;
using System.Threading.Tasks;

namespace app.Controllers
{
    // Modelo de dados para Livro
    public class Livro
    {
        public int Id { get; set; }
        [Required]
        public string Autor { get; set; }
        [Required]
        public int NumeroPaginas { get; set; }
        [Required]
        public string ISBN { get; set; }
        public string Nome { get; set; }
        public int QuantidadeVendas { get; set; }
        [Range(0, 5)]
        public double Avaliacao { get; set; }
    }

    // Contexto do banco de dados
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options)
        {
        }

        public DbSet<Livro> Livros { get; set; }
    }

    // Controller para Livros
    [Route("api/[controller]")]
    [ApiController]
    public class LivrosController : ControllerBase
    {
        private readonly ApplicationDbContext _context;

        public LivrosController(ApplicationDbContext context)
        {
            _context = context;
        }

        // GET: api/Livros
        [HttpGet]
        public async Task<ActionResult<IEnumerable<Livro>>> GetLivros()
        {
            return await _context.Livros.ToListAsync();
        }

        // Método adicional para configurar Entity Framework no Startup.cs
        public static void ConfigureEntityFramework(IServiceCollection services, string connectionString)
        {
            services.AddDbContext<ApplicationDbContext>(options =>
                options.UseSqlServer(connectionString));
        }
    }
}
```{{copy}}

E vamos adicionar um trecho de código na linha 9 do arquivo `Program.cs`:

```cs
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

```{{copy}}

E adicionar ao topo do arquivo `Program.cs`:
```cs

using Microsoft.EntityFrameworkCore;
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using app.Controllers;

```{{copy}}

E também vamos precisar instalar algumas dependências. Para isso, vamos executar novamente o container docker como executamos no primeiro passo:

`docker run --rm -v ./:/app --workdir=/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:8.0`{{execute}}

E vamos adicionar as dependências:

`dotnet add package Microsoft.EntityFrameworkCore.SqlServer`{{execute}}

Pronto! Agora podemos **sair do container**, **compilar nossa imagem** docker e **executar o docker-compose** (com docker-compose up -d)!

Porta 8080: [Acesso ao WebAPI de Livros]({{TRAFFIC_HOST1_8080}}/api/Livros)

*PS: nada deve acontecer, pois os dados ainda não estão no banco!*