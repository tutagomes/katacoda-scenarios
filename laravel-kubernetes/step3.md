Agora que está tudo executando, vamos expor nosso serviço para o mundo externo!

Primeiro, será necessário executar o comando de configurar a chave da aplicação Laravel:

`kubectl exec -ti deployment/exemplo-laravel bash`{{execute}}

`php artisan key:generate`{{execute}}

`exit`{{execute}}



Segundo, vamos executar um comando para expor as portas:

`kubectl expose deployment exemplo-laravel --port=80 --type=LoadBalancer`{{execute}}

`kubectl port-forward deployment/exemplo-laravel 9000:80 --address 0.0.0.0`{{execute}}

E agora, podemos acessar nosso serviço através da URL:



https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com/api/cafes

https://[[HOST_SUBDOMAIN]]-9000-[[KATACODA_HOST]].environments.katacoda.com



