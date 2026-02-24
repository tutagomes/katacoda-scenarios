## Mapeamento de Portas

Por padrão, um container roda de forma **completamente isolada** — ele tem sua própria rede interna e não é acessível de fora. Para que uma aplicação dentro do container possa ser acessada, precisamos criar uma "ponte" entre uma porta do container e uma porta da nossa máquina.

Isso é feito com a flag `-p`:

```
-p PORTA_DA_MAQUINA_HOST:PORTA_DO_CONTAINER
```

O Docker cria uma regra de redirecionamento de rede: toda requisição que chegar na porta `8080` da máquina host é encaminhada para a porta `80` dentro do container.

### Rodando com porta exposta

`docker run --name nginx-web -d -p 8080:80 nginx`{{execute}}

Verifique os containers:

`docker ps`{{execute}}

Observe que agora aparece `0.0.0.0:8080->80/tcp` na coluna PORTS, confirmando o mapeamento. O prefixo `0.0.0.0` significa que a porta está vinculada a todas as interfaces de rede da máquina host — acessível tanto localmente quanto de outros computadores na mesma rede.

Vamos testar o acesso:

`curl localhost:8080`{{execute}}

Ou acesse pelo browser: [Abrir na porta 8080]({{TRAFFIC_HOST1_8080}})

### Múltiplos containers, mesma imagem

Podemos rodar vários containers da mesma imagem simultaneamente, desde que cada um use uma porta diferente na máquina:

`docker run --name nginx-web2 -d -p 8081:80 nginx`{{execute}}

`docker ps`{{execute}}

`curl localhost:8081`{{execute}}

[Abrir na porta 8081]({{TRAFFIC_HOST1_8081}})

> Tentar subir dois containers na mesma porta da máquina causaria um conflito — a porta `8081` já está ocupada e o sistema operacional não permite duas ligações à mesma porta simultaneamente.
