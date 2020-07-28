Finalmente, podemos executar os serviços e visualizá-los através dos comandos:

`docker stack deploy --compose-file docker-compose.yml exemplo`{{execute}}

`docker stack ls`{{execute}}

`docker stack services exemplo`{{execute}}

E claro, ao navegarmos até a URL abaixo, podemos ver além do Hello World, que o ID do contêiner também é mostrado.
Experimente entrar novamente e analisar se o ID é o mesmo ou se você foi redirecionado para uma nova instância!
Porta 4000: https://[[HOST_SUBDOMAIN]]-4000-[[KATACODA_HOST]].environments.katacoda.com/
