## Containers se comunicando

Agora vamos simular uma situação real: uma aplicação web que precisa consultar outro serviço. Vamos usar dois containers nginx — um representando uma API e outro um cliente que faz requisições a ela.

### Criando a rede do projeto

`docker network create rede-projeto`{{execute}}

### Subindo o container "API"

`docker run -d --name api --network rede-projeto nginx`{{execute}}

### Verificando que a API está no ar

`docker exec api curl -s localhost`{{execute}}

### Subindo o container "cliente" e acessando a API pelo nome

Agora vamos subir um container com `curl` disponível e acessar a API usando apenas o **nome do container**:

`docker run -d --name cliente --network rede-projeto alpine sleep 3600`{{execute}}

`docker exec cliente apk add --no-cache curl`{{execute}}

`docker exec cliente curl -s http://api`{{execute}}

O cliente acessou o servidor nginx digitando apenas `api` — o Docker resolveu o nome para o IP correto. Esse é exatamente o padrão usado no mundo real:

- Container `app` acessa container `db` em `db:5432`
- Container `app` acessa container `cache` em `cache:6379`
- Nenhum IP hardcoded, nenhuma configuração frágil

### Múltiplos nomes (aliases)

Você pode dar um apelido a um container dentro de uma rede específica:

`docker network connect --alias servidor-web rede-projeto api`{{execute}}

`docker exec cliente curl -s http://servidor-web`{{execute}}

O mesmo container agora responde tanto por `api` quanto por `servidor-web` dentro dessa rede.
