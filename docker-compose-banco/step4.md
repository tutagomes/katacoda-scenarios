## Aguardando o banco ficar pronto

O `depends_on` garante que o container do SQL Server sobe antes da API — mas não que o SQL Server esteja pronto para aceitar conexões. O SQL Server leva alguns segundos para inicializar internamente, e se a API tentar conectar antes, ela falha.

A solução correta é usar `healthcheck` com `condition: service_healthy`:

<pre class="file" data-filename="docker-compose.yml" data-target="replace">
services:
  api:
    build: .
    container_name: tarefas-api
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
      - ConnectionStrings__Default=Server=db;Database=TarefasDb;User=sa;Password=Senha@123;TrustServerCertificate=True
    depends_on:
      db:
        condition: service_healthy
    restart: on-failure

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: tarefas-db
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=Senha@123
    volumes:
      - dados-sqlserver:/var/opt/mssql
    healthcheck:
      test: ["CMD-SHELL", "/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P Senha@123 -No -Q 'SELECT 1' || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 10
      start_period: 30s

volumes:
  dados-sqlserver:
</pre>

### O que mudou

**`healthcheck` no serviço `db`:**
Executa um `SELECT 1` a cada 10 segundos. O SQL Server é considerado `healthy` quando esse comando retorna com sucesso. O `start_period: 30s` dá um tempo inicial antes de começar a testar — evitando falsos negativos durante a inicialização.

**`depends_on` com `condition: service_healthy`:**
A API só sobe quando o banco reportar saúde.

**`restart: on-failure`:**
Se a API falhar na inicialização (ex: banco ainda não pronto), ela tenta novamente automaticamente.

### Aplicando as mudanças

`docker compose down`{{execute}}

`docker compose up -d --build`{{execute}}

`docker compose ps`{{execute}}

Observe o SQL Server passando por `starting → healthy` antes de a API aparecer como `running`.

`docker compose logs -f db`{{execute}}

Pressione `Ctrl+C` quando ver que o SQL Server está pronto.
