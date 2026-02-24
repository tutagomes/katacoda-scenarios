## Inspecionando redes

Saber diagnosticar problemas de rede é tão importante quanto criar a rede certa. Vamos montar um mini cenário e usar as ferramentas de inspeção do Docker para entendê-lo.

### Montando o cenário

`docker network create rede-app`{{execute}}

`docker run -d --name api --network rede-app nginx`{{execute}}

`docker run -d --name db --network rede-app redis:alpine`{{execute}}

`docker run -d --name monitor alpine sleep 3600`{{execute}}

Note que `monitor` não foi conectado à `rede-app` — isso é proposital.

### Listando todas as redes

`docker network ls`{{execute}}

### Inspecionando uma rede em detalhe

O `docker network inspect` mostra tudo sobre uma rede — configuração de IP, containers conectados, endereços atribuídos:

`docker network inspect rede-app`{{execute}}

Procure a seção `"Containers"` — ela lista `api` e `db` com seus IPs internos. O `monitor` não aparece aqui porque está na rede `bridge` padrão.

### Verificando a qual rede um container pertence

`docker inspect api --format='{{json .NetworkSettings.Networks}}' | python3 -m json.tool`{{execute}}

Você verá que `api` está conectado à `rede-app` com seu IP.

### Diagnosticando: "por que meu container não alcança o outro?"

Vamos simular o diagnóstico mais comum. O `monitor` tenta acessar a `api`:

`docker exec monitor wget -q -T 3 -O- http://api 2>&1 || echo "Falhou: monitor não alcança api"`{{execute}}

**Passo 1 — verificar em qual rede cada container está:**

`docker inspect monitor --format='Redes: {{range $k, $v := .NetworkSettings.Networks}}{{$k}} {{end}}'`{{execute}}

`docker inspect api --format='Redes: {{range $k, $v := .NetworkSettings.Networks}}{{$k}} {{end}}'`{{execute}}

O `monitor` está na `bridge`, a `api` está na `rede-app` — redes diferentes, não se enxergam.

**Passo 2 — resolver conectando à mesma rede:**

`docker network connect rede-app monitor`{{execute}}

`docker exec monitor wget -q -O- http://api > /dev/null && echo "Agora funciona!"`{{execute}}

### Desconectando um container de uma rede

`docker network disconnect rede-app monitor`{{execute}}

`docker inspect monitor --format='Redes: {{range $k, $v := .NetworkSettings.Networks}}{{$k}} {{end}}'`{{execute}}

O `monitor` voltou a ter acesso apenas à `bridge` padrão.

### Removendo redes

Para remover uma rede, todos os containers devem estar desconectados primeiro:

`docker rm -f api db monitor`{{execute}}

`docker network rm rede-app`{{execute}}

Para remover todas as redes que não têm nenhum container ativo:

`docker network prune -f`{{execute}}

`docker network ls`{{execute}}

Sobram apenas as três redes padrão — `bridge`, `host` e `none`.
