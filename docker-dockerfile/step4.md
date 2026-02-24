## Fazendo o build

Com o Dockerfile pronto, agora vamos construir a imagem. Certifique-se de estar na pasta com os arquivos:

`ls -l`{{execute}}

Você deve ver `package.json`, `server.js` e `Dockerfile`. Agora execute o build:

`docker build -t minha-app:1.0 .`{{execute}}

Vamos entender o comando:

| Parte | Significado |
|---|---|
| `docker build` | Constrói uma imagem a partir de um Dockerfile |
| `-t minha-app:1.0` | Define o nome (`minha-app`) e a tag/versão (`1.0`) da imagem |
| `.` | O contexto do build — a pasta atual onde o Docker vai procurar o Dockerfile e os arquivos |

### O que acontece durante o build

Observe a saída do comando. Cada linha do Dockerfile gera um **Step** numerado:

```
Step 1/7 : FROM node:alpine
Step 2/7 : WORKDIR /usr/src/app
Step 3/7 : COPY package*.json ./
Step 4/7 : RUN npm install
...
```

Cada step gera uma **camada** (*layer*) na imagem. As camadas são empilhadas, e o resultado final é a sua imagem.

### O poder do cache

Rode o build novamente:

`docker build -t minha-app:1.0 .`{{execute}}

Perceba que desta vez é muito mais rápido! Você vai ver `---> Using cache` na maioria dos steps. Como nenhum arquivo mudou, o Docker reutilizou todas as camadas do cache.

Agora veja a imagem criada:

`docker images`{{execute}}

> **Nomeando imagens:** A tag `:1.0` é importante para controle de versão. Você pode fazer um novo build com `:2.0` e manter ambas as versões disponíveis. A tag `:latest` é atribuída automaticamente quando você não especifica uma.
