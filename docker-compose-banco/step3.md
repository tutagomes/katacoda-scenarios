## Volumes nomeados no Compose

Agora vamos criar o `docker-compose.yml` com o SQL Server e um **volume nomeado** para persistir os dados.

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
      - db

  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: tarefas-db
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=Senha@123
    volumes:
      - dados-sqlserver:/var/opt/mssql

volumes:
  dados-sqlserver:
</pre>

### Entendendo os volumes nomeados

A seção `volumes:` no final do arquivo declara o volume `dados-sqlserver`. O Docker o cria automaticamente na primeira execução e o gerencia independentemente dos containers.

A linha `- dados-sqlserver:/var/opt/mssql` monta esse volume no diretório onde o SQL Server armazena seus dados. Tudo que o SQL Server grava ali é persistido no volume — não no container.

```
Container db (efêmero)
    ↕ montado em /var/opt/mssql
Volume dados-sqlserver (persistente)
    ↕ gerenciado pelo Docker no host
/var/lib/docker/volumes/dados-sqlserver/
```

### Subindo a aplicação

O SQL Server demora um pouco para inicializar. Vamos subir e aguardar:

`docker compose up -d --build`{{execute}}

`sleep 30`{{execute}}

`docker compose ps`{{execute}}

### Listando volumes criados

`docker volume ls`{{execute}}

`docker volume inspect <pasta>_dados-sqlserver`{{execute}}

O campo `Mountpoint` mostra onde no host os dados estão armazenados fisicamente.
