## Multi-stage builds

O maior salto em otimização vem dos **multi-stage builds** — uma funcionalidade do Docker que permite usar múltiplos `FROM` em um único Dockerfile. Cada `FROM` inicia um novo estágio, e você pode copiar apenas os artefatos necessários de um estágio para outro.

### Por que isso importa

Em muitas linguagens (Go, Java, TypeScript, Rust), o processo de build gera um artefato compilado. As ferramentas de compilação são pesadas, mas o resultado final é leve. Sem multi-stage, você acaba com um container de produção cheio de compiladores que nunca serão usados.

```
Estágio de BUILD:          Estágio de PRODUÇÃO:
- node:latest              - node:alpine
- npm install (todas)      - apenas dependências de produção
- typescript, webpack...   - apenas o código compilado
- testes, linters...
         ↓
   Copia só o necessário
```

### Simulando um build com transpilação

Vamos simular uma aplicação que precisa de uma etapa de "build" antes de rodar:

`touch Dockerfile.otimizado`{{execute}}

```Dockerfile
# ── Estágio 1: dependências e build ──────────────────────────────
FROM node:latest AS builder

WORKDIR /build

# Instala TODAS as dependências (incluindo devDependencies)
COPY package*.json ./
RUN npm install

# Copia o código e "compila" (aqui apenas copia, mas simula a etapa)
COPY server.js .

# ── Estágio 2: imagem final de produção ──────────────────────────
FROM node:alpine AS producao

WORKDIR /usr/src/app

# Copia apenas o package.json e instala só dependências de produção
COPY package*.json ./
RUN npm install --omit=dev

# Copia o artefato gerado pelo estágio de build
COPY --from=builder /build/server.js .

EXPOSE 3000

CMD ["node", "server.js"]
```{{copy}}


### Fazendo o build multi-stage

`docker build -f Dockerfile.otimizado -t app-multistage .`{{execute}}

A flag `-f` especifica um Dockerfile com nome diferente do padrão.

### Testando que funciona

`docker run --name app-final -d -p 3000:3000 app-multistage`{{execute}}

`curl localhost:3000`{{execute}}

### Você também pode construir apenas um estágio específico

Isso é útil para rodar testes em CI antes de fazer o build de produção:

`docker build -f Dockerfile.otimizado --target builder -t app-builder .`{{execute}}

`docker images app-builder`{{execute}}
