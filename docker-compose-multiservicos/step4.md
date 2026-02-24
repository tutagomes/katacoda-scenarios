## Ordem de inicialização

O Docker Compose sobe todos os serviços em paralelo por padrão. Isso pode causar problemas: se a API tentar conectar ao Redis antes dele estar pronto, o container da API falha na inicialização.

### O problema

Derrube tudo e observe o que acontece ao subir:

`docker compose down`{{execute}}

`docker compose up`{{execute}}

Às vezes a API sobe antes do Redis estar completamente pronto e você vê erros de conexão. O `depends_on` resolve isso.

### depends_on básico

A forma mais simples garante que o container do Redis seja **criado** antes da API — mas não que o Redis esteja **pronto para receber conexões**:

<pre class="file" data-filename="docker-compose.yml" data-target="replace">
services:
  api:
    build: .
    container_name: contador-api
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
      - REDIS_HOST=cache
    networks:
      - rede-app
    depends_on:
      - cache

  cache:
    image: redis:alpine
    container_name: contador-cache
    networks:
      - rede-app

networks:
  rede-app:
    driver: bridge
</pre>

### depends_on com healthcheck

Para garantir que o Redis esteja realmente aceitando conexões antes de subir a API, usamos `depends_on` com `condition: service_healthy` junto de um `healthcheck`:

<pre class="file" data-filename="docker-compose.yml" data-target="replace">
services:
  api:
    build: .
    container_name: contador-api
    ports:
      - "8080:8080"
    environment:
      - ASPNETCORE_URLS=http://+:8080
      - REDIS_HOST=cache
    networks:
      - rede-app
    depends_on:
      cache:
        condition: service_healthy

  cache:
    image: redis:alpine
    container_name: contador-cache
    networks:
      - rede-app
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 5

networks:
  rede-app:
    driver: bridge
</pre>

O `healthcheck` executa `redis-cli ping` a cada 5 segundos. O Compose só considera o Redis saudável (`healthy`) quando o comando retorna `PONG`. Só então a API é iniciada.

`docker compose down`{{execute}}

`docker compose up -d --build`{{execute}}

`docker compose ps`{{execute}}

Observe a coluna `Status` — o Redis vai de `starting` para `healthy` antes de a API subir.
