Para criar nossa primeira imagem docker hospedando uma aplicação NodeJS, vamos então criar uma pequena aplicação em JS. Começando pelas dependências:

<pre class="file" data-filename="package.json" data-target="replace">{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
</pre>

E finalmente para o conteúdo. Aqui, vamos executar um pequeno servidor HTTP (Express) para servir uma mensagem de 'Hello World!' quando acessarmos a página inicial:

<pre class="file" data-filename="server.js" data-target="replace">'use strict';
const express = require('express');
const os = require('os');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/', (req, res) => {
  res.send('Hello World from ' + os.hostname() + '!');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);
</pre>

E para facilitar, já vamos colocar tudo dentro de um "container" para que possamos instalar as dependências necessárias e empacotar a aplicão!

<pre class="file" data-filename="Dockerfile" data-target="replace">FROM node:alpine

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "node", "server.js" ]
</pre>

Agora, basta executar o comando para realizar a build da imagem! A ferramenta irá seguir todos os passos definidos no arquivo anterior, começando por baixar a imagem Node com a versão 12, copiar o package.json, instalar as dependências e finalmente definir uma porta e um script de entrada para nossa imagem. O nome (ou tag) `nodelocal` é meramente ilustrativo. Poderíamos colocar qualquer outra.

`docker build -t nodelocal .`{{execute}}
