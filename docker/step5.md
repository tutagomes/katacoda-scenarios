Agora com a imagem criada, podemos visualizá-la através do comando:

`docker images`{{execute}}

E então, podemos executá-la através do comando:

`docker run --name mynodeapp -d -p 9090:8080 nodelocal` {{execute}}

Porta 9090: https://[[HOST_SUBDOMAIN]]-9090-[[KATACODA_HOST]].environments.katacoda.com/

E para pará-lo, basta executar o comando:

`docker stop mynodeapp`{{execute}}