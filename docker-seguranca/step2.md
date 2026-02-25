## Criando um usuário não-root

A solução mais direta para o problema do root é criar um usuário dedicado dentro da imagem e usá-lo para rodar a aplicação.

### Criando a aplicação

Primeiro, vamos criar uma aplicação simples para usar nos testes:

`touch server.js`{{execute}}

```js
'use strict';
const http = require('http');
const os = require('os');

const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end(`Rodando como: ${os.userInfo().username}\n`);
});

server.listen(3000, '0.0.0.0');
console.log('Rodando na porta 3000');
```{{copy}}

### O Dockerfile com usuário não-root

`touch Dockerfile`{{execute}}

```Dockerfile
FROM node:alpine

WORKDIR /usr/src/app

COPY server.js .

# Cria um grupo e um usuário sem privilégios
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Garante que o usuário tem acesso à pasta da aplicação
RUN chown -R appuser:appgroup /usr/src/app

# A partir daqui, tudo roda como appuser
USER appuser

EXPOSE 3000

CMD ["node", "server.js"]
```{{copy}}

Vamos entender as instruções de segurança:

| Instrução | O que faz |
|---|---|
| `addgroup -S appgroup` | Cria um grupo de sistema (sem senha, sem home) |
| `adduser -S appuser -G appgroup` | Cria um usuário de sistema vinculado ao grupo |
| `chown -R appuser:appgroup /usr/src/app` | Dá ao usuário acesso à pasta da aplicação |
| `USER appuser` | Define que todos os comandos seguintes rodam como esse usuário |

> As flags `-S` criam usuários/grupos de **sistema** — sem shell de login, sem diretório home, sem senha. Exatamente o perfil de um usuário que só precisa rodar um processo.

### Fazendo o build e testando

`docker build -t app-segura .`{{execute}}

`docker run -d --name app-segura -p 3000:3000 app-segura`{{execute}}

Verifique quem está rodando o processo:

`docker exec app-segura whoami`{{execute}}

`docker exec app-segura id`{{execute}}

Agora é `appuser` com `uid` diferente de 0. Vamos confirmar que a aplicação funciona normalmente:

`curl localhost:3000`{{execute}}

### Verificando que os privilégios foram reduzidos

Tente sobrescrever um arquivo de sistema:

`docker exec app-segura sh -c "echo 'invadido' > /etc/hostname" 2>&1 || echo "Permissão negada: como esperado"`{{execute}}

Tente instalar pacotes:

`docker exec app-segura sh -c "apk add curl" 2>&1 || echo "Permissão negada: como esperado"`{{execute}}

Ambos falham — o `appuser` não tem privilégios para isso. Se um atacante comprometer a aplicação, ele herda essas mesmas limitações.

### Definindo o usuário no docker run (sem alterar o Dockerfile)

Nem sempre você tem controle sobre a imagem. Nesses casos, use a flag `--user` no momento de rodar:

`docker run --rm --user 1000:1000 alpine id`{{execute}}

O container roda com `uid=1000` e `gid=1000`, independente do que está no Dockerfile.

### Limpando

`docker rm -f app-segura`{{execute}}

> **Resumo:** `USER` no Dockerfile é a forma mais robusta de garantir que sua aplicação nunca rode como root. Para imagens de terceiros, use `--user` no `docker run`.
