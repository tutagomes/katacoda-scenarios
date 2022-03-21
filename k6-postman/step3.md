
Vamos executar nosso primeiro teste de carga. De maneira bem simples, se acessar o docker-compose.yaml, é possível ver que o nosso serviço do app está limitado a utilizar 1 CPU e 200MB de RAM. Assim, podemos dimensionar nosso teste para que este hardware consiga responder todos nossos usuários.

Para começar, vamos converter nossa coleção de testes Postman para executá-las com o k6 através do comando:

`npx postman-to-k6 Postman-Teste-V1.json -o k6-v1-script.js`{{execute}}

Teremos então um script pronto para ser rodado. O próximo passo é dimensionar a quantidade de usuários e o tempo que nosso teste irá durar. Assim, podemos medir quantas requisições são completadas em, por exemplo, 30 segundos e validar se houve algum problema durante o processo.

Vamos primeiro executar um teste de Warmup para já iniciar as conexões com o banco, abrir os canais de comunicação com a aplicação e já carregar em memória o código a ser executado:

`k6 run --vus 2 --duration 10s k6-v1-script.js`{{execute}}

Executemos então um teste com 5 usuários durante 30 segundos com o comando:

`k6 run --vus 5 --duration 30s k6-v1-script.js -o influxdb=http://localhost:8086/k6`{{execute}}

Quem sabe com mais usuários?

`k6 run --vus 15 --duration 30s k6-v1-script.js -o influxdb=http://localhost:8086/k6`{{execute}}

E então, analisamos o resultado na nossa inteface Grafana

https://[[HOST_SUBDOMAIN]]-3030-[[KATACODA_HOST]].environments.katacoda.com

E claro, não precisamos utilizar somente uma dashboard, podemos adicionar várias outras montadas pela comunidade!

Como por exemplo:

- [k6 Load Testing Results by km](https://grafana.com/grafana/dashboards/10660)

- [k6 Load Testing Resultsby smockvavelsky](https://grafana.com/grafana/dashboards/10553)

- [k6 Load Testing Resultsby Stian Øvrevåge](https://grafana.com/grafana/dashboards/4411)

[E como eu importo uma dashboard?](https://grafana.com/docs/grafana/latest/dashboards/export-import/#importing-a-dashboard)