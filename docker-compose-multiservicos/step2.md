## Adicionando o Redis

Agora vamos criar o `docker-compose.yml` com dois serviços: a API e o Redis.

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

  cache:
    image: redis:alpine
    container_name: contador-cache
</pre>

### O que acontece aqui

**Dois serviços, duas responsabilidades:**
- `api`: nossa aplicação .NET, construída a partir do Dockerfile
- `cache`: o Redis, usando a imagem oficial do Docker Hub

**A mágica da rede automática:**
O Compose cria automaticamente uma rede privada e conecta ambos os serviços a ela. Dentro dessa rede, cada container é resolvível pelo **nome do serviço**.

Por isso `REDIS_HOST=cache` funciona: a API acessa o Redis simplesmente usando o hostname `cache` — que é o nome do serviço no Compose.

```
container "contador-api"  →  redis.Connect("cache")  →  container "contador-cache"
```

Não há configuração manual de rede, não há IPs — o Compose cuida de tudo.

### Subindo os dois serviços

`docker compose up -d --build`{{execute}}

`docker compose ps`{{execute}}

Aguarde ambos aparecerem com status `running`.

### Testando o contador

`curl http://localhost:8080`{{execute}}

`curl http://localhost:8080`{{execute}}

`curl http://localhost:8080`{{execute}}

Cada requisição incrementa o contador no Redis. [Abrir no browser]({{TRAFFIC_HOST1_8080}})

### Resetando o contador

`curl -X DELETE http://localhost:8080/resetar`{{execute}}

`curl http://localhost:8080`{{execute}}

O contador voltou a 1!
