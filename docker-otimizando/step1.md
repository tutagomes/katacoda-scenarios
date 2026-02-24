## Por que o tamanho importa

Antes de otimizar, vamos entender o problema. Vamos criar uma imagem intencionalmente ruim e medir seu impacto.

### Criando uma aplicação simples

<pre class="file" data-filename="server.js" data-target="replace">
'use strict';
const http = require('http');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Olá, Docker otimizado!\n');
});

server.listen(3000, '0.0.0.0');
console.log('Rodando na porta 3000');
</pre>

<pre class="file" data-filename="package.json" data-target="replace">
{
  "name": "app-otimizacao",
  "version": "1.0.0",
  "main": "server.js",
  "dependencies": {
    "express": "^4.18.0"
  }
}
</pre>

### Um Dockerfile sem boas práticas

<pre class="file" data-filename="Dockerfile" data-target="replace">
FROM node:latest

WORKDIR /app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "server.js"]
</pre>

### Fazendo o build e medindo o tamanho

`docker build -t app-sem-otimizacao .`{{execute}}

`docker images app-sem-otimizacao`{{execute}}

Anote o tamanho da imagem. Provavelmente está acima de **900MB** — para uma aplicação que tem apenas dois arquivos e faz uma coisa simples!

### Visualizando as camadas e o que engorda a imagem

`docker history app-sem-otimizacao`{{execute}}

O `docker history` mostra cada camada e seu tamanho. Repare que a imagem base `node:latest` já vem com centenas de MB antes de você adicionar qualquer coisa.

Nos próximos passos vamos reduzir drasticamente esse tamanho.
