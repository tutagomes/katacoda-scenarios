## Criando uma rede customizada

Redes customizadas do tipo `bridge` têm uma vantagem fundamental sobre a rede padrão: elas incluem um **servidor DNS interno** que permite que containers se encontrem pelo nome. Você nunca mais precisa gerenciar IPs manualmente.

### Criando a rede

`docker network create minha-rede`{{execute}}

Verifique que ela foi criada:

`docker network ls`{{execute}}

### Um cenário real: aplicação web + cache

Vamos simular algo comum no mundo real — um servidor web que precisa se comunicar com um Redis (cache/banco em memória):

`docker run -d --name redis --network minha-rede redis:alpine`{{execute}}

`docker run -d --name web --network minha-rede alpine sleep 3600`{{execute}}

Agora, de dentro do container `web`, acesse o Redis **pelo nome**:

`docker exec web wget -q -O- http://redis:6379 2>&1 || echo "Conexão recebida pelo Redis (porta respondeu)"`{{execute}}

O Docker resolveu automaticamente o nome `redis` para o IP correto. Esse é o DNS interno em ação — sem IPs hardcoded, sem configuração extra.

Na prática, é exatamente assim que sua aplicação acessaria o banco de dados ou o cache:

- Container `app` acessa container `db` em `db:5432`
- Container `app` acessa container `cache` em `cache:6379`
- Nenhum IP hardcoded, nenhuma configuração frágil

### Testando com nginx

Vamos adicionar um nginx à mesma rede para ver a resolução de nomes com uma resposta HTTP completa:

`docker run -d --name servidor --network minha-rede nginx`{{execute}}

`docker exec web wget -q -O- http://servidor`{{execute}}

O container `web` conseguiu acessar o nginx digitando apenas `servidor`.

### Conectando um container existente a uma rede

Também é possível conectar um container que já está rodando a uma rede:

`docker run -d --name outro-servidor nginx`{{execute}}

Esse container não está em `minha-rede` — vamos conectá-lo:

`docker network connect minha-rede outro-servidor`{{execute}}

`docker exec web wget -q -O- http://outro-servidor`{{execute}}

### Aliases: múltiplos nomes para o mesmo container

Você pode dar apelidos a um container dentro de uma rede. Há duas formas:

**No `docker run` com `--network-alias`:**

```bash
docker run -d --name api-v2 --network minha-rede --network-alias api nginx
```

`docker run -d --name api-v2 --network minha-rede --network-alias api nginx`{{execute}}

Agora `api-v2` responde tanto por `api-v2` quanto por `api`:

`docker exec web wget -q -O- http://api`{{execute}}

**No `docker network connect` com `--alias`:**

```bash
docker network connect --alias servidor-web minha-rede outro-servidor
```

`docker network connect --alias servidor-web minha-rede outro-servidor`{{execute}}

`docker exec web wget -q -O- http://servidor-web`{{execute}}

Aliases são úteis para criar nomes semânticos (`api`, `cache`, `db`) independente do nome real do container.

### Limpando

`docker rm -f redis web servidor outro-servidor api-v2`{{execute}}

`docker network rm minha-rede`{{execute}}
