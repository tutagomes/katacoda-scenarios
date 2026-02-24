## Escrevendo o Dockerfile

Agora vamos criar o Dockerfile que vai transformar nossa aplicação em uma imagem Docker.

<pre class="file" data-filename="Dockerfile" data-target="replace">
FROM node:alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD ["node", "server.js"]
</pre>

### Entendendo cada linha

**`FROM node:alpine`**
Começa a partir da imagem oficial do Node.js com Alpine Linux. O Alpine é uma distribuição minimalista (~5MB), perfeita para manter a imagem final pequena.

**`WORKDIR /usr/src/app`**
Define `/usr/src/app` como o diretório de trabalho dentro do container. Se o diretório não existir, o Docker o cria automaticamente. Todos os comandos seguintes serão executados a partir daqui.

**`COPY package*.json ./`**
Copia apenas os arquivos `package.json` e `package-lock.json` (se existir) para o container. O `*` garante que ambos sejam copiados.

**`RUN npm install`**
Instala as dependências declaradas no `package.json`. Esta etapa é a mais demorada do build.

> **Por que copiar o `package.json` separado antes do código?**
> Porque o Docker guarda cache por camada. Se você copiar tudo de uma vez (`COPY . .`) e mudar qualquer arquivo do código, o Docker vai reinstalar todas as dependências do zero. Separando o `COPY package*.json` antes do `RUN npm install`, o Docker só reinstala as dependências quando o `package.json` mudar — e reutiliza o cache quando só o código mudou. Isso acelera bastante os builds do dia a dia.

**`COPY . .`**
Agora copia o restante dos arquivos da aplicação (incluindo `server.js`) para dentro do container.

**`EXPOSE 8080`**
Documenta que a aplicação escuta na porta 8080. Lembre-se: isso não publica a porta — é apenas uma declaração para quem for usar a imagem.

**`CMD ["node", "server.js"]`**
Define o comando que será executado quando o container iniciar. O formato de array `["node", "server.js"]` é o recomendado pois evita que o comando seja interpretado por um shell intermediário.
