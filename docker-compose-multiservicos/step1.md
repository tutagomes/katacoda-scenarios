## A API com contador de visitas

Vamos criar uma API .NET que usa o Redis para contar quantas vezes foi acessada. O Redis é um banco de dados em memória muito usado para cache, sessões e contadores — e é um exemplo perfeito de serviço de suporte.

### O projeto com dependência do Redis

<pre class="file" data-filename="MinhaApi.csproj" data-target="replace">
&lt;Project Sdk="Microsoft.NET.Sdk.Web"&gt;
  &lt;PropertyGroup&gt;
    &lt;TargetFramework&gt;net8.0&lt;/TargetFramework&gt;
    &lt;Nullable&gt;enable&lt;/Nullable&gt;
    &lt;ImplicitUsings&gt;enable&lt;/ImplicitUsings&gt;
  &lt;/PropertyGroup&gt;
  &lt;ItemGroup&gt;
    &lt;PackageReference Include="StackExchange.Redis" Version="2.7.4" /&gt;
  &lt;/ItemGroup&gt;
&lt;/Project&gt;
</pre>

### A API

<pre class="file" data-filename="Program.cs" data-target="replace">
using StackExchange.Redis;

var builder = WebApplication.CreateBuilder(args);

var redisHost = builder.Configuration["REDIS_HOST"] ?? "localhost";
var redis = ConnectionMultiplexer.Connect(redisHost);
builder.Services.AddSingleton<IConnectionMultiplexer>(redis);

var app = builder.Build();

app.MapGet("/", async (IConnectionMultiplexer redis) =>
{
    var db = redis.GetDatabase();
    var visitas = await db.StringIncrementAsync("visitas");
    return Results.Ok(new
    {
        mensagem = "Olá!",
        totalVisitas = visitas
    });
});

app.MapGet("/saude", () => Results.Ok(new { status = "ok" }));
app.MapDelete("/resetar", async (IConnectionMultiplexer redis) =>
{
    var db = redis.GetDatabase();
    await db.KeyDeleteAsync("visitas");
    return Results.Ok(new { mensagem = "Contador resetado!" });
});

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

Note que a API lê o host do Redis a partir da variável de configuração `REDIS_HOST`. No próximo passo vamos ver como o Docker Compose fornece esse valor automaticamente.
