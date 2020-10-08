Vamos então partir para a execução!

Para começar, vamos executar o nosso Laravel e o MySQL, preparando-os em seguida! Aqui, precisamos executar o comando docker-compose up. Mas perceba que passamos alguns parâmetros diferentes (como limite de memória e CPU)


`docker-compose --compatibility up -d`{{execute}}

O resultado do comando deve ser:

```bash
Creating network "network-todo" with driver "bridge"
Creating laravel-locust-test_database_1 ... done
Creating laravel-locust-test_laravel_1  ... done
```


Vamos ver se está tudo certo?

`docker ps`{{execute}}

O resultado deve ser:

```bash
controlplane $ kubectl get deployments
CONTAINER ID        IMAGE                               COMMAND                  CREATED             STATUS              PORTS                                            NAMES
553816e1d7f2        tutagomes/laravel-loadtest:latest   "docker-php-entrypoi…"   1 minutes ago      Up 1 minutes       0.0.0.0:8082->80/tcp                             laravel-locust-test_laravel_1
0aa772609557        mysql:latest                        "docker-entrypoint.s…"   1 minutes ago      Up 1 minutes       3306/tcp, 33060/tcp                              laravel-locust-test_database_1
```

Caso queira ver também se a rede foi criada com sucesso, pode executar o comando

`docker network ls`{{execute}}