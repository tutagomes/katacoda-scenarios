## A API com SQL Server

Vamos criar uma API .NET 8 de gerenciamento de tarefas usando Entity Framework Core com SQL Server.

### O projeto

<pre class="file" data-filename="MinhaApi.csproj" data-target="replace">
&lt;Project Sdk="Microsoft.NET.Sdk.Web"&gt;
  &lt;PropertyGroup&gt;
    &lt;TargetFramework&gt;net8.0&lt;/TargetFramework&gt;
    &lt;Nullable&gt;enable&lt;/Nullable&gt;
    &lt;ImplicitUsings&gt;enable&lt;/ImplicitUsings&gt;
  &lt;/PropertyGroup&gt;
  &lt;ItemGroup&gt;
    &lt;PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.0" /&gt;
  &lt;/ItemGroup&gt;
&lt;/Project&gt;
</pre>

### A API com CRUD de Tarefas

<pre class="file" data-filename="Program.cs" data-target="replace">
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

var connString = builder.Configuration.GetConnectionString("Default")
    ?? "Server=db;Database=TarefasDb;User=sa;Password=Senha@123;TrustServerCertificate=True";

builder.Services.AddDbContext&lt;AppDbContext&gt;(opt =>
    opt.UseSqlServer(connString));

var app = builder.Build();

// Cria o banco automaticamente na primeira execução
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService&lt;AppDbContext&gt;();
    db.Database.EnsureCreated();
}

app.MapGet("/tarefas", async (AppDbContext db) =>
    await db.Tarefas.ToListAsync());

app.MapGet("/tarefas/{id}", async (int id, AppDbContext db) =>
    await db.Tarefas.FindAsync(id) is Tarefa t
        ? Results.Ok(t)
        : Results.NotFound());

app.MapPost("/tarefas", async (Tarefa tarefa, AppDbContext db) =>
{
    db.Tarefas.Add(tarefa);
    await db.SaveChangesAsync();
    return Results.Created($"/tarefas/{tarefa.Id}", tarefa);
});

app.MapPut("/tarefas/{id}", async (int id, Tarefa input, AppDbContext db) =>
{
    var tarefa = await db.Tarefas.FindAsync(id);
    if (tarefa is null) return Results.NotFound();
    tarefa.Titulo = input.Titulo;
    tarefa.Concluida = input.Concluida;
    await db.SaveChangesAsync();
    return Results.NoContent();
});

app.MapDelete("/tarefas/{id}", async (int id, AppDbContext db) =>
{
    var tarefa = await db.Tarefas.FindAsync(id);
    if (tarefa is null) return Results.NotFound();
    db.Tarefas.Remove(tarefa);
    await db.SaveChangesAsync();
    return Results.NoContent();
});

app.Run();

class Tarefa
{
    public int Id { get; set; }
    public string Titulo { get; set; } = string.Empty;
    public bool Concluida { get; set; }
}

class AppDbContext : DbContext
{
    public AppDbContext(DbContextOptions&lt;AppDbContext&gt; options) : base(options) { }
    public DbSet&lt;Tarefa&gt; Tarefas =&gt; Set&lt;Tarefa&gt;();
}
</pre>

### O Dockerfile

<pre class="file" data-filename="Dockerfile" data-target="replace">
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY *.csproj .
RUN dotnet restore
COPY . .
RUN dotnet publish -c Release -o /app

FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app .
EXPOSE 8080
ENTRYPOINT ["dotnet", "MinhaApi.dll"]
</pre>
