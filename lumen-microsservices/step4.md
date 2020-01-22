Agora com o ambiente já pronto, podemos então iniciar o monitoramento dos conteineres utilizando Prometheus + Graphana.

Para começar, vamos na mesma pasta executar o comando git clone para recuperar um repositório já pronto:

`git clone https://github.com/vegasbrianc/prometheus.git`{{execute}}

Vamos navegar então até a pasta:

`cd prometheus`{{execute}}

E iniciar então um Swarm:

`docker swarm init`{{execute}}

Agora sim podemos fazer o deploy da nossa aplicação de monitoramento:

`docker stack deploy -c docker-stack.yml prom`{{execute}}
 
 É possível analisar que diferente do docker-compose up, a saída dos serviços não são mostradas no console, já que o comando deploy funciona de uma forma bem distante do compose.