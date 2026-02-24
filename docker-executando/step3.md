## Mapeamento de Portas

Por padrão, um container roda de forma **completamente isolada** — ele tem sua própria rede interna e não é acessível de fora. Para que uma aplicação dentro do container possa ser acessada, precisamos criar uma "ponte" entre uma porta do container e uma porta da nossa máquina.

Isso é feito com a flag `-p`:

```
-p PORTA_DA_SUA_MAQUINA:PORTA_DO_CONTAINER
```

Pense assim: é como configurar um interfone que redireciona a campainha da porta `80` do container para a porta `8080` da sua máquina. Quem bater na `8080` de fora vai ser atendido pela `80` dentro do container.

### Rodando com porta exposta

`docker run --name nginx-web -d -p 8080:80 nginx`{{execute}}

Verifique os containers:

`docker ps`{{execute}}

Observe que agora aparece `0.0.0.0:8080->80/tcp` na coluna PORTS, confirmando o mapeamento.

Vamos testar o acesso:

`curl localhost:8080`{{execute}}

Ou acesse pelo browser: [Abrir na porta 8080]({{TRAFFIC_HOST1_8080}})

### Múltiplos containers, mesma imagem

Podemos rodar vários containers da mesma imagem simultaneamente, desde que cada um use uma porta diferente na máquina:

`docker run --name nginx-web2 -d -p 8081:80 nginx`{{execute}}

`docker ps`{{execute}}

`curl localhost:8081`{{execute}}

[Abrir na porta 8081]({{TRAFFIC_HOST1_8081}})

> Tentar subir dois containers na mesma porta da máquina causaria um erro — é como tentar ligar dois telefones no mesmo número ao mesmo tempo.
