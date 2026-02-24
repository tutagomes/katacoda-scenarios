## Boas práticas no Dockerfile

Além do `.dockerignore`, existem boas práticas na escrita do próprio Dockerfile que reduzem tamanho e melhoram o cache.

### 1. Use imagens base menores

A primeira e mais impactante otimização é trocar a imagem base:

| Imagem | Tamanho aproximado |
|---|---|
| `node:latest` | ~950MB |
| `node:slim` | ~250MB |
| `node:alpine` | ~50MB |

O Alpine Linux é uma distribuição minimalista com apenas o essencial para rodar sua aplicação.

### 2. Copie o package.json antes do código

Já vimos isso no cenário de criação de imagens, mas vale reforçar aqui como prática de otimização de build:

```dockerfile
# ✅ Certo — npm install só roda de novo se o package.json mudar
COPY package*.json ./
RUN npm install
COPY . .

# ❌ Errado — qualquer mudança no código invalida o cache do npm install
COPY . .
RUN npm install
```

### 3. Combine RUNs relacionados

Cada instrução `RUN` cria uma camada. Camadas de limpeza em `RUN`s separados não eliminam o arquivo das camadas anteriores:

```dockerfile
# ❌ Errado — o arquivo existe na camada anterior mesmo após o rm
RUN apt-get update
RUN apt-get install -y curl
RUN rm -rf /var/lib/apt/lists/*

# ✅ Certo — tudo na mesma camada, o arquivo nunca existiu nas camadas anteriores
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*
```

### Aplicando as boas práticas

<pre class="file" data-filename="Dockerfile" data-target="replace">
FROM node:alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install --omit=dev

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
</pre>

Note também o `--omit=dev`: instala apenas dependências de produção, excluindo ferramentas de desenvolvimento.

`docker build -t app-boas-praticas .`{{execute}}

`docker images`{{execute}}

Compare o tamanho de `app-sem-otimizacao` vs `app-boas-praticas`. A redução deve ser considerável só com essas mudanças.
