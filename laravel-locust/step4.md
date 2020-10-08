E agora, como subimos o Locust?

Uma alternativa viável seria instalar o locust através dos comandos
```bash
pip3 install locust
```

Mas como estamos em ambiente virtualizado e sabemos que já existe um contêiner pronto para nós, basta executar o comando:


`docker run -p 8089:8089 -v $PWD/locust:/mnt/locust --network=network-todo locustio/locust -f /mnt/locust/teste_exemplo.py -H http://laravel-locust-test_laravel_1:80`{{execute}}

> Note que o contêiner é executado já na rede interna dos nossos serviços, para facilitar a comunicação e não precisar ficar passando por vários resolutores de DNS


E claro, como tudo é mais emocionante com uma interface, podemos acessá-la através do link:

https://[[HOST_SUBDOMAIN]]-8089-[[KATACODA_HOST]].environments.katacoda.com

