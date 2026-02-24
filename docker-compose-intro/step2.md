## Criando a API .NET

Vamos criar uma API .NET 8 mínima para usar como base do nosso cenário.

### O projeto

<pre class="file" data-filename="MinhaApi.csproj" data-target="replace">
&lt;Project Sdk="Microsoft.NET.Sdk.Web"&gt;
  &lt;PropertyGroup&gt;
    &lt;TargetFramework&gt;net8.0&lt;/TargetFramework&gt;
    &lt;Nullable&gt;enable&lt;/Nullable&gt;
    &lt;ImplicitUsings&gt;enable&lt;/ImplicitUsings&gt;
  &lt;/PropertyGroup&gt;
&lt;/Project&gt;
</pre>

### O código da API

<pre class="file" data-filename="Program.cs" data-target="replace">
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => Results.Ok(new
{
    mensagem = "Olá do Docker Compose!",
    ambiente = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") ?? "Unknown",
    versao = "1.0"
}));

app.MapGet("/saude", () => Results.Ok(new { status = "ok" }));

app.Run();
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

Este Dockerfile usa o padrão multi-stage que você viu no cenário de otimização: o estágio `build` usa o SDK completo para compilar, e o estágio `runtime` usa apenas o ASP.NET runtime — muito mais leve.

### Verificando os arquivos criados

`ls -la`{{execute}}

No próximo passo vamos escrever o `docker-compose.yml` para orquestrar esse serviço.
