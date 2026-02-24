## Override para desenvolvimento

O arquivo `docker-compose.override.yml` é **carregado automaticamente** pelo Docker Compose sempre que você rodar `docker compose up` — sem precisar passá-lo com `-f`. É o lugar ideal para configurações de desenvolvimento.

<pre class="file" data-filename="docker-compose.override.yml" data-target="replace">
services:
  api:
    container_name: tarefas-api-dev
    build:
      context: ./app
      target: build          # usa o estágio de build com SDK completo
    volumes:
      - ./app:/src           # monta o código local para ver mudanças
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__Default=Server=db;Database=TarefasDb;User=sa;Password=DevSenha@123;TrustServerCertificate=True

  db:
    container_name: tarefas-db-dev
    environment:
      - SA_PASSWORD=DevSenha@123
    ports:
      - "1433:1433"          # expõe o banco para ferramentas locais (DBeaver, SSMS)
</pre>

### O que o override adiciona em dev

**`target: build`** — usa o estágio de build do Dockerfile (com SDK completo) em vez do runtime. Isso permite hot-reload com `dotnet watch`.

**`volumes: - ./app:/src`** — monta o código-fonte local dentro do container. Qualquer mudança no código é refletida imediatamente sem rebuild.

**`ports: "1433:1433"` no banco** — expõe o SQL Server para que ferramentas como DBeaver ou Azure Data Studio possam conectar diretamente do seu computador.

**Senha simples** — em dev usamos senhas previsíveis para facilitar. Em produção isso muda.

### Como funciona a mesclagem

O Compose **mescla** os dois arquivos automaticamente. As chaves que existem apenas no override são adicionadas; as que existem nos dois são sobrescritas pelo override.

`docker compose config`{{execute}}

O comando `config` mostra a configuração final mesclada — exatamente o que o Compose vai usar.

### Subindo em modo dev

`docker compose up -d --build`{{execute}}

`docker compose ps`{{execute}}
