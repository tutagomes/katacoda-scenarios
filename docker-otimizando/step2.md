## Boas práticas no Dockerfile

No step anterior vimos que uma imagem simples pode passar de 1GB. A maior parte desse peso vem de escolhas no Dockerfile que podemos melhorar.

### 1. Use imagens base menores

A primeira e mais impactante otimização é trocar a imagem base:

| Imagem | Tamanho aproximado |
|---|---|
| `node:latest` | ~1GB |
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

Vamos reescrever o Dockerfile aplicando tudo que vimos: imagem base Alpine (muito menor que `node:latest`), `package.json` copiado separadamente para aproveitar o cache, e `--omit=dev` para instalar apenas dependências de produção.

```Dockerfile
FROM node:alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install --omit=dev

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
```{{copy}}

Note também o `--omit=dev`: instala apenas dependências de produção, excluindo ferramentas de desenvolvimento.

`docker build -t app-boas-praticas .`{{execute}}

`docker images`{{execute}}

Compare o tamanho de `app-sem-otimizacao` vs `app-boas-praticas`. A redução deve ser considerável só com essas mudanças.
