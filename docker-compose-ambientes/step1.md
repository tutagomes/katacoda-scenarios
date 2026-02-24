## O arquivo base

O arquivo `docker-compose.yml` define tudo que é **comum a todos os ambientes**: os serviços que existem, as imagens, as redes e os volumes. Ele nunca deve conter configurações específicas de dev ou produção.

Vamos usar a API de tarefas do cenário anterior como base. Primeiro, crie os arquivos da aplicação:

`mkdir -p app`{{execute}}

`cat > app/MinhaApi.csproj << 'EOF'`{{execute}}
`<Project Sdk="Microsoft.NET.Sdk.Web">`{{execute}}
`  <PropertyGroup>`{{execute}}
`    <TargetFramework>net8.0</TargetFramework>`{{execute}}
`    <Nullable>enable</Nullable>`{{execute}}
`    <ImplicitUsings>enable</ImplicitUsings>`{{execute}}
`  </PropertyGroup>`{{execute}}
`  <ItemGroup>`{{execute}}
`    <PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer" Version="8.0.0" />`{{execute}}
`  </ItemGroup>`{{execute}}
`</Project>`{{execute}}
`EOF`{{execute}}

Agora o arquivo base do Compose:

<pre class="file" data-filename="docker-compose.yml" data-target="replace">
services:
  api:
    build: ./app
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
    depends_on:
      db:
        condition: service_healthy
    restart: on-failure
    networks:
      - rede-app

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      - ACCEPT_EULA=Y
    volumes:
      - dados-sqlserver:/var/opt/mssql
    healthcheck:
      test: ["CMD-SHELL", "/opt/mssql-tools18/bin/sqlcmd -S localhost -U sa -P $${SA_PASSWORD} -No -Q 'SELECT 1' || exit 1"]
      interval: 10s
      timeout: 5s
      retries: 10
      start_period: 30s
    networks:
      - rede-app

networks:
  rede-app:

volumes:
  dados-sqlserver:
</pre>

### O que está faltando intencionalmente

Repare que o arquivo base **não tem**:
- A senha do banco (`SA_PASSWORD`) — varia por ambiente
- A connection string da API — depende da senha
- Configuração de `container_name` — pode conflitar em CI/CD

Essas configurações ficarão nos arquivos de override.
