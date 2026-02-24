## ARG vs ENV

Além do `ENV`, o Dockerfile tem outra instrução para variáveis: o `ARG`. Eles parecem similares, mas têm propósitos e momentos completamente diferentes.

| | `ARG` | `ENV` |
|---|---|---|
| **Quando existe** | Somente durante o `docker build` | Durante o build e em tempo de execução |
| **Visível no container** | Não | Sim |
| **Pode ser sobrescrito** | Com `--build-arg` | Com `-e` no `docker run` |
| **Uso típico** | Versão de dependências, URLs de download | Configurações da aplicação, credenciais |

### Usando ARG para parametrizar o build

Um uso clássico de `ARG` é controlar a versão da imagem base sem alterar o Dockerfile:

<pre class="file" data-filename="Dockerfile" data-target="replace">
FROM node:alpine

# ARG define a variável só para o momento do build
ARG APP_VERSION=1.0.0
ARG BUILD_DATE

# ENV torna a variável disponível dentro do container
ENV APP_VERSION=$APP_VERSION
ENV BUILD_DATE=$BUILD_DATE
ENV PORT=3000
ENV APP_ENV=development
ENV MENSAGEM="Olá do Dockerfile!"

WORKDIR /usr/src/app
COPY server.js .

EXPOSE $PORT
CMD ["node", "server.js"]
</pre>

### Passando ARGs no build

`docker build -t app-variaveis:v2 --build-arg APP_VERSION=2.0.0 --build-arg BUILD_DATE=2026-01-01 .`{{execute}}

### Confirmando que ARG não vaza para o container

`docker run --rm app-variaveis:v2 env | grep BUILD`{{execute}}

`docker run --rm app-variaveis:v2 env | grep APP_VERSION`{{execute}}

Note que `BUILD_DATE` (que ficou apenas como `ARG`) **não aparece** no container. Já `APP_VERSION` aparece porque foi transferida para um `ENV`. Isso é importante por segurança: nunca passe segredos via `ARG` pensando que estarão protegidos — eles ficam visíveis no histórico da imagem.

`docker history app-variaveis:v2`{{execute}}

Veja que os valores dos `ARG` aparecem no histórico da imagem. Por esse motivo, **nunca use ARG para senhas ou tokens**.
