## Escrevendo o docker-compose.yml

Agora vamos criar o arquivo que define como nossa API deve ser executada:

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
</pre>

### Entendendo cada parte

**`services:`**
A raiz onde todos os containers da aplicação são definidos. Aqui temos apenas um: `api`.

**`build: .`**
Instrui o Compose a construir a imagem a partir do `Dockerfile` na pasta atual (`.`). Também poderíamos usar `image: nginx` para usar uma imagem pronta do Hub.

**`container_name: minha-api`**
Define um nome fixo para o container. Sem isso, o Compose gera um nome automático como `pasta_api_1`.

**`ports:`**
Mapeia portas no formato `HOST:CONTAINER` — igual ao `-p` do `docker run`.

**`environment:`**
Define variáveis de ambiente. `ASPNETCORE_URLS` diz ao .NET em qual endereço e porta escutar.

### Subindo a aplicação

`docker compose up`{{execute}}

Observe o output: o Compose primeiro faz o build da imagem (você verá os steps do Dockerfile), depois inicia o container. As linhas de log aparecem em tempo real.

Abra uma nova aba do terminal e acesse a API:

[Abrir na porta 8080]({{TRAFFIC_HOST1_8080}})

Ou via curl:

`curl http://localhost:8080`{{execute}}

Volte ao primeiro terminal e pressione `Ctrl+C` para parar.
