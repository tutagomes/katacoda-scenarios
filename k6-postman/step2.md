Agora, com a imagem pronta para ser executada, podemos iniciar nossos serviços no modo `detached`, passando um parâmetro `-d` para o nosso `docker-compose`

`docker-compose --compatibility up -d`{{execute}}

Assim, os serviços são executados em segundo plano e podemos utilizar o mesmo terminal para executar os comandos para testes.

Neste passo, vamos também instalar o comando:

`npm install -g postman-to-k6`{{execute}}

Se quiser validar o funcionamento na nossa API, basta entrar na URL:

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/v1/users

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/v2/users

---

Vamos também instalar algumas ferramentas de visualização, para ficar mais fácil explorar nossos resultados ao longo dos testes:

`git clone https://github.com/loadimpact/k6`{{execute}}
`cd k6`{{execute}}

E vamos trocar a porta do Grafana para não gerar conflito com nossa aplicação:

`sed -i 's/3000:3000/3030:3000/' docker-compose.yml`{{execute}}

E então, podemos executar nossa aplicação
`docker-compose up -d influxdb grafana`{{execute}}
E acessá-la através da URL
https://[[HOST_SUBDOMAIN]]-3030-[[KATACODA_HOST]].environments.katacoda.com