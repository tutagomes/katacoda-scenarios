Vamos executar nosso segundo teste de carga, desta vez, apontando para a versão 2 da nossa API.

Para começar, vamos converter nossa coleção de testes Postman para executá-las com o k6 através do comando:

`postman-to-k6 Postman-Teste-V2.json -o k6-v2-script.js`{{execute}}

Vamos primeiro executar um teste de Warmup para já iniciar as conexões com o banco, abrir os canais de comunicação com a aplicação e já carregar em memória o código a ser executado:

`k6 run --vus 2 --duration 10s k6-v2-script.js`{{execute}}

Executemos então um teste com 5 usuários durante 30 segundos com o comando:

`k6 run --vus 5 --duration 30s k6-v2-script.js -o influxdb=http://localhost:8086/k6`{{execute}}

E então, analisamos o resultado na nossa interface Grafana:
https://[[HOST_SUBDOMAIN]]-3030-[[KATACODA_HOST]].environments.katacoda.com

Perguntas a serem respondidas após o teste:

- A performance foi maior ou menor?
- Em qual ambiente devo rodar para atingir 2000 RPS?
- A última alteração do código/da infraestrutura adicionou latência?
- Falha em mais requisições?
