## Inspecionando redes

Saber diagnosticar problemas de rede é tão importante quanto criar a rede certa. O Docker oferece algumas ferramentas úteis para isso.

### Listando todas as redes

`docker network ls`{{execute}}

### Inspecionando uma rede em detalhe

O `docker network inspect` mostra tudo sobre uma rede: configuração de IP, containers conectados, endereços atribuídos:

`docker network inspect rede-back`{{execute}}

Procure a seção `"Containers"` — ela lista todos os containers atualmente conectados com seus endereços IP internos.

### Verificando a qual rede um container pertence

`docker inspect api-gateway | grep -A 20 '"Networks"'`{{execute}}

Você verá que o `api-gateway` aparece conectado a duas redes (`rede-front` e `rede-back`), cada uma com seu próprio IP.

### Desconectando um container de uma rede

`docker network disconnect rede-back api-gateway`{{execute}}

`docker inspect api-gateway | grep -A 20 '"Networks"'`{{execute}}

Agora o `api-gateway` pertence apenas à `rede-front`.

### Removendo redes

Para remover uma rede, todos os containers devem estar desconectados dela primeiro:

`docker rm -f front back api-gateway`{{execute}}

`docker network rm rede-front rede-back`{{execute}}

`docker network ls`{{execute}}

> O Docker não permite remover redes que ainda têm containers conectados — isso é uma proteção para evitar desconexões acidentais.

### Removendo redes não utilizadas de uma vez

`docker network prune`{{execute}}

O `prune` remove todas as redes que não têm nenhum container ativo — útil para limpar o ambiente de desenvolvimento.
