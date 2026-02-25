## ENV no Dockerfile

Até agora passamos variáveis no `docker run`. Mas também é possível definir variáveis **dentro do Dockerfile** usando a instrução `ENV`. Elas se tornam parte da imagem e ficam disponíveis em todos os containers criados a partir dela.

### Criando uma aplicação que lê variáveis

Vamos criar uma aplicação Node.js que usa variáveis de ambiente para se configurar:

```sh
touch server.js
```{{copy}}

```js
'use strict';
const http = require('http');

const PORT = process.env.PORT || 3000;
const APP_ENV = process.env.APP_ENV || 'development';
const MENSAGEM = process.env.MENSAGEM || 'Olá, Mundo!';

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/html' });
  res.end(`
    <h1>${MENSAGEM}</h1>
    <p><strong>Ambiente:</strong> ${APP_ENV}</p>
    <p><strong>Porta interna:</strong> ${PORT}</p>
  `);
});

server.listen(PORT, '0.0.0.0', () => {
  console.log(`Rodando em modo ${APP_ENV} na porta ${PORT}`);
});
```{{copy}}

### Definindo valores padrão com ENV

```sh
touch Dockerfile
```{{copy}}

```Dockerfile
FROM node:alpine

WORKDIR /usr/src/app

COPY server.js .

ENV PORT=3000
ENV APP_ENV=development
ENV MENSAGEM="Olá do Dockerfile!"

EXPOSE $PORT

CMD ["node", "server.js"]
```{{copy}}

Note que usamos `EXPOSE $PORT` — o Docker substitui o valor da variável no momento do build.

### Fazendo o build e rodando

`docker build -t app-variaveis .`{{execute}}

`docker run --name app-default -d -p 3000:3000 app-variaveis`{{execute}}

[Abrir na porta 3000]({{TRAFFIC_HOST1_3000}})

### Sobrescrevendo o valor padrão em tempo de execução

Os valores do `ENV` no Dockerfile são apenas **padrões** — eles podem ser sobrescritos com `-e` no `docker run`:

`docker run --name app-prod -d -p 3001:3001 -e PORT=3001 -e APP_ENV=producao -e MENSAGEM="Bem-vindo à produção!" app-variaveis`{{execute}}

[Abrir na porta 3001]({{TRAFFIC_HOST1_3001}})

Mesma imagem, comportamento diferente. Isso é o poder das variáveis de ambiente.
