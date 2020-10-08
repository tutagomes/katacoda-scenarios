Agora que está tudo executando, vamos começar a executar alguns comandos dentro do nosso contêiner laravel, como aplicar as migrações e realizar alguns links:


`docker exec -ti laravel-locust-test_laravel_1 /bin/bash`{{execute}}

`php artisan migrate`{{execute}}
`php artisan key:generate`{{execute}}
`php artisan config:cache`{{execute}}
`php artisan storage:link`{{execute}}

`exit`{{execute}}


E agora, podemos acessar nosso serviço através da URL:



https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com

https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/docs

Também temos algumas APIs já expostas


https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/api/v1/todo
https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/api/v2/todo
https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/api/v3/todo

E claro, uma API para exemplificar um login
https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/api/login
