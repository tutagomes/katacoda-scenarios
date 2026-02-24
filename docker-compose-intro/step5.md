## Variáveis e rebuild

### Alterando variáveis de ambiente

Vamos adicionar uma variável personalizada ao `docker-compose.yml`:

<pre class="file" data-filename="docker-compose.yml" data-target="replace">
services:
  api:
    build: .
    container_name: minha-api
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ASPNETCORE_URLS=http://+:8080
      - APP_VERSAO=2.0
      - APP_AUTOR=Arthur
</pre>

E vamos ler essas variáveis no código:

<pre class="file" data-filename="Program.cs" data-target="replace">
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapGet("/", () => Results.Ok(new
{
    mensagem = "Olá do Docker Compose!",
    ambiente = Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT"),
    versao = Environment.GetEnvironmentVariable("APP_VERSAO"),
    autor = Environment.GetEnvironmentVariable("APP_AUTOR")
}));

app.MapGet("/saude", () => Results.Ok(new { status = "ok" }));

app.Run();
</pre>

### Reconstruindo após mudanças no código

Simplesmente fazer `docker compose up -d` novamente **não** reconstrói a imagem — ela é reutilizada do cache. Para forçar o rebuild:

`docker compose up -d --build`{{execute}}

A flag `--build` garante que o Compose reconstrói a imagem antes de subir o container.

Teste a mudança:

`curl http://localhost:8080`{{execute}}

### Executando um comando dentro de um serviço

Equivalente ao `docker exec`, mas usando o nome do serviço:

`docker compose exec api env`{{execute}}

### Limpando o ambiente

`docker compose down`{{execute}}
