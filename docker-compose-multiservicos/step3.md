## Comunicação entre serviços

Vamos explorar mais a fundo como os serviços se enxergam dentro da rede criada pelo Compose.

### Confirmando a rede criada automaticamente

`docker network ls`{{execute}}

O Compose cria uma rede com o nome no formato `<pasta>_default`. Todos os serviços do arquivo são conectados a ela automaticamente.

### Inspecionando a rede

`docker network inspect $(docker network ls --filter name=default --format "{{.Name}}" | head -1)`{{execute}}

Na seção `Containers` você verá os dois containers com seus IPs internos atribuídos.

### Provando a resolução de nomes

Vamos entrar na API e testar se ela consegue alcançar o Redis pelo nome:

`docker compose exec api sh -c "wget -qO- http://cache:6379 || echo 'Redis nao fala HTTP, mas o DNS funcionou!'"`{{execute}}

Ou de forma mais direta, verificar a resolução DNS:

`docker compose exec api sh -c "nslookup cache"`{{execute}}

O hostname `cache` resolve para o IP interno do container Redis.

### Criando uma rede com nome customizado

Se preferir uma rede com nome explícito em vez do `_default`, você pode declará-la:

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

  cache:
    image: redis:alpine
    container_name: contador-cache
    networks:
      - rede-app

networks:
  rede-app:
    driver: bridge
</pre>

`docker compose down`{{execute}}

`docker compose up -d --build`{{execute}}

`docker network ls`{{execute}}

Agora a rede tem um nome legível em vez do sufixo `_default`.
