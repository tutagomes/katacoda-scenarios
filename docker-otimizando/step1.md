## Por que o tamanho importa

Antes de otimizar, vamos entender o problema. Vamos criar uma imagem intencionalmente ruim e medir seu impacto.

### Criando uma aplicação simples

`touch server.js`{{execute}}

```js
'use strict';
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Olá, Docker otimizado!\n');
});

server.listen(3000, '0.0.0.0');
console.log('Rodando na porta 3000');
```{{copy}}


`touch package.json`{{execute}}

```json
{
  "name": "meu-server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "type": "commonjs",
  "dependencies": {
    "express": "^5.2.1"
  },
  "devDependencies": {
    "@biomejs/biome": "2.4.4",
    "jest": "^30.2.0",
    "nodemon": "^3.1.14"
  }
}
```{{copy}}

### Um Dockerfile sem boas práticas

`touch Dockerfile`{{execute}}


```Dockerfile
FROM node:latest

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "server.js"]
```{{copy}}

### Fazendo o build e medindo o tamanho

`docker build -t app-sem-otimizacao .`{{execute}}

`docker images `{{execute}}

Anote o tamanho da imagem. Provavelmente está acima de **1GB** — para uma aplicação que tem apenas dois arquivos e faz uma coisa simples!

### Visualizando as camadas e o que engorda a imagem

`docker history app-sem-otimizacao`{{execute}}

O `docker history` mostra cada camada e seu tamanho. Repare que a imagem base `node:latest` já vem com centenas de MB antes de você adicionar qualquer coisa.

Nos próximos passos vamos reduzir drasticamente esse tamanho.
