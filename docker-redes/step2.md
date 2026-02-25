## Criando uma rede customizada

Redes customizadas do tipo `bridge` têm uma vantagem fundamental sobre a rede padrão: elas incluem um **servidor DNS interno** que permite que containers se encontrem pelo nome. Você nunca mais precisa gerenciar IPs manualmente.

### Criando a rede

`docker network create minha-rede`{{execute}}

Verifique que ela foi criada:

`docker network ls`{{execute}}

### Subindo containers na rede

Use a flag `--network` para conectar um container à rede no momento da criação:

`docker run -d --name servidor --network minha-rede nginx`{{execute}}

`docker run -d --name web --network minha-rede alpine sleep 3600`{{execute}}

> O `sleep 3600` mantém o container `alpine` vivo por 1 hora sem fazer nada — útil para containers de teste que precisam ficar disponíveis para comandos manuais.

### Testando a resolução de nomes

Agora, de dentro do container `web`, acesse o nginx usando apenas o **nome do container**:

`docker exec web wget -q -O- http://servidor`{{execute}}

Funcionou! O Docker resolveu automaticamente o nome `servidor` para o IP do container correto. Esse é o DNS interno em ação — sem IPs hardcoded, sem configuração extra.

Na prática, é exatamente assim que containers se comunicam no mundo real:

- Container `app` acessa container `db` em `db:5432`
- Container `app` acessa container `cache` em `cache:6379`
- Nenhum IP hardcoded, nenhuma configuração frágil

### Conectando um container existente a uma rede

Também é possível conectar um container que já está rodando a uma rede:

`docker run -d --name outro-servidor nginx`{{execute}}

Esse container não está em `minha-rede` — vamos conectá-lo:

`docker network connect minha-rede outro-servidor`{{execute}}

`docker exec web wget -q -O- http://outro-servidor`{{execute}}

### Aliases: múltiplos nomes para o mesmo container

Você pode dar apelidos a um container dentro de uma rede. Há duas formas:

**No `docker run` com `--network-alias`:**

`docker run -d --name api-v2 --network minha-rede --network-alias api nginx`{{execute}}

Agora `api-v2` responde tanto por `api-v2` quanto por `api`:

`docker exec web wget -q -O- http://api`{{execute}}

**No `docker network connect` com `--alias`:**

```bash
docker network connect --alias servidor-web minha-rede outro-servidor
```{{execute}}

`docker exec web wget -q -O- http://servidor-web`{{execute}}

Aliases são úteis para criar nomes semânticos (`api`, `cache`, `db`) independente do nome real do container.

### Limpando

`docker rm -f web servidor outro-servidor api-v2`{{execute}}

`docker network rm minha-rede`{{execute}}
