## Containers se comunicando

Agora vamos simular uma situação real: uma aplicação web que precisa consultar outro serviço. Vamos usar dois containers nginx — um representando uma API e outro um cliente que faz requisições a ela.

### Criando a rede do projeto

`docker network create rede-projeto`{{execute}}

### Subindo o container "API"

`docker run -d --name api --network rede-projeto nginx`{{execute}}

### Verificando que a API está no ar

`docker exec api wget -q -O- http://localhost`{{execute}}

### Subindo o container "app" e acessando a API pelo nome

O Alpine já vem com `wget` — sem precisar instalar nada. Vamos usá-lo para acessar a API usando apenas o **nome do container**:

`docker run -d --name app --network rede-projeto alpine sleep 3600`{{execute}}

`docker exec app wget -q -O- http://api`{{execute}}

O container `app` acessou o nginx digitando apenas `api` — o Docker resolveu o nome para o IP correto. Esse é exatamente o padrão usado no mundo real:

- Container `app` acessa container `db` em `db:5432`
- Container `app` acessa container `cache` em `cache:6379`
- Nenhum IP hardcoded, nenhuma configuração frágil

### Múltiplos nomes (aliases)

Você pode conectar um container a uma rede com um apelido adicional, permitindo que ele seja acessado por mais de um nome:

`docker run -d --name api-v2 nginx`{{execute}}

`docker network connect --alias servidor-web rede-projeto api-v2`{{execute}}

`docker exec app wget -q -O- http://servidor-web`{{execute}}

O container `api-v2` agora responde por `api-v2` e também por `servidor-web` dentro da `rede-projeto`. Isso é útil para criar aliases de versão ou nomes semânticos para serviços.
