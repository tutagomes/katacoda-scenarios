Por padrão, o Locust com a interface web habilitada, não inicia os testes de forma automática. Para inciá-los, é necessário especificar a quantidade de usuários e a velocidade que eles serão criados.

Para nosso primeiro teste de warmup, vamos executar com poucos usuários, 5 apenas e com 1 segundo de spawn.

Assim, já será possível visualizar alguns dos dados plotados nos gráficos. Note que as tasks são executadas por cada usuário de forma cíclica, sempre iniciando um novo fluxo quando o anterior acaba.

E agora, é a hora de começarmos a melhorar nossos testes. Para isso, pare o contêiner com CTRL+C. Sempre que necessário, pode subí-lo novamente com o comando:

`docker run -p 8089:8089 -v $PWD/locust:/mnt/locust --network=network-todo locustio/locust -f /mnt/locust/teste_exemplo.py -H http://teste-carga_laravel_1:80`{{execute}}

E acessar a interface através da URL:
https://[[HOST_SUBDOMAIN]]-8089-[[KATACODA_HOST]].environments.katacoda.com

