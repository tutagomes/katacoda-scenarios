Neste passo vamos então executar a aplicação e tentar recuperar algum dado através de uma requisição GET apontando para nosso gateway.

Para iniciar os serviços, vamos executar o comando:
`docker-compose up`{{execute}}

Ao executar, veremos que a saída dos serviços é diretamente apontada para o console, o que pode não ser ideal em alguns cenários.
Para isso, vamos adicionar uma flag à execução, parando-a primeiro com CTRL + C

`docker-compose up -d`{{execute}}

Desta forma o Docker é executado de forma desconectada com o terminal vigente.

E claro, o resultado pode ser conferido aqui: https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/users

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/lugares

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/avaliacoes