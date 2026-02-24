## Criando a aplicação

Vamos criar uma pequena aplicação Node.js que será o conteúdo da nossa imagem Docker. Ela vai expor um servidor HTTP simples que responde com uma mensagem ao ser acessado.

### Definindo as dependências

Crie o arquivo `package.json` com as dependências da aplicação:

<pre class="file" data-filename="package.json" data-target="replace">
{
  "name": "minha-app-docker",
  "version": "1.0.0",
  "description": "Minha primeira aplicação em um container Docker",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.18.0"
  }
}
</pre>

### Criando o servidor

Agora crie o arquivo `server.js`:

<pre class="file" data-filename="server.js" data-target="replace">
'use strict';

const express = require('express');
const os = require('os');

const PORT = 8080;
const HOST = '0.0.0.0';

const app = express();

app.get('/', (req, res) => {
  res.send(`
    <h1>Olá, Docker!</h1>
    <p>Esta resposta veio de dentro do container.</p>
    <p><strong>Hostname do container:</strong> ${os.hostname()}</p>
  `);
});

app.listen(PORT, HOST, () => {
  console.log(`Servidor rodando em http://${HOST}:${PORT}`);
});
</pre>

O `os.hostname()` retorna o ID do container — isso vai ser interessante de ver quando rodarmos a aplicação.

### O que temos até agora

- `package.json`: declara que nossa aplicação depende do Express
- `server.js`: um servidor HTTP que responde na rota `/` com uma página HTML simples

No próximo passo, vamos criar o Dockerfile para empacotar tudo isso em uma imagem.
