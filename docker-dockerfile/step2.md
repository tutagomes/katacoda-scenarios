## Criando a aplicação

Vamos criar uma pequena aplicação Node.js que será o conteúdo da nossa imagem Docker. Ela vai expor um servidor HTTP simples que responde com uma mensagem ao ser acessado.

### Definindo as dependências

Crie o arquivo `package.json` com as dependências da aplicação:

```
touch package.json
```{{exec}}

```
{
  "name": "minha-app-docker",
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
  }
}
```{{copy}}

### Criando o servidor

Agora crie o arquivo `server.js` com o conteúdo:

```
touch server.js
```{{exec}}

````js
'use strict';

const express = require('express');
const os = require('os');

const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();

app.get('/', (req, res) => {
  console.log('Requisição recebida em', new Date().toISOString());
  res.send(`
    <h1>Olá, Docker!</h1>
    <p>Esta resposta veio de dentro do container.</p>
    <p><strong>Hostname do container:</strong> ${os.hostname()}</p>
  `);
});

app.listen(PORT, HOST, () => {
  console.log(`Servidor rodando em http://${HOST}:${PORT}`);
});
````{{copy}}

O `os.hostname()` retorna o ID do container — isso vai ser interessante de ver quando rodarmos a aplicação.

### O que temos até agora

- `package.json`: declara que nossa aplicação depende do Express
- `server.js`: um servidor HTTP que responde na rota `/` com uma página HTML simples

No próximo passo, vamos criar o Dockerfile para empacotar tudo isso em uma imagem.
