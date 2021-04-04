Antes de continuar nossa jornada, vamos executar o nosso servidor backend, responsável por organizar o banco e servir APIs que podemos consumir na nossa interface:

Já existem algumas coleções preparadas para executarmos nosso exemplo, mas se preferir, pode sempre gerar uma nova no [Mockaroo](https://www.mockaroo.com/). Vamos clonar o repositório:

`git clone https://github.com/tutagomes/json-mock-collection.git`{{execute "T2"}}


Navegar até a pasta baixada

`cd json-mock-collection`{{execute "T2"}}

E executar nosso servidor

`json-server --watch db_clients_30.json --host 0.0.0.0`{{execute "T2"}}

E agora, podemos navegar até a URL e validar o funcionamento. Ela deve retornar um texto JSON com 30 clientes!

https://[[HOST_SUBDOMAIN]]-3000-[[KATACODA_HOST]].environments.katacoda.com/clients

> E guarde essa URL, precisaremos dela para configurar nossa aplicação!
