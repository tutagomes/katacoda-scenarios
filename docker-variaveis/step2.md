## Usando um arquivo .env

Quando uma aplicação precisa de muitas variáveis, passar cada uma com `-e` se torna impraticável. A solução é usar um arquivo `.env` — uma convenção amplamente adotada no ecossistema Docker.

### Criando o arquivo .env
```
touch .env
```{{exec}}

```env
APP_ENV=development
APP_PORT=8080
APP_NAME=Minha Aplicação
DB_HOST=localhost
DB_PORT=5432
DB_NAME=meu_banco
DB_USER=admin
DB_PASSWORD=senha-de-dev
LOG_LEVEL=debug
```{{copy}}

### Usando com --env-file

Em vez de passar variável por variável, basta apontar para o arquivo:

`docker run --rm --env-file .env alpine env`{{execute}}

Todas as variáveis do arquivo são injetadas no container de uma vez.

### .env não é incluído automaticamente

Um ponto importante: diferente do `docker-compose`, o `docker run` **não** lê o `.env` automaticamente. Você precisa passar `--env-file` explicitamente.

Isso é na verdade uma proteção: evita que variáveis sensíveis sejam expostas por acidente.

### Mantendo o .env fora do repositório

O arquivo `.env` frequentemente contém senhas e chaves de API. **Nunca commite ele no Git.** A prática correta é:

1. Adicionar `.env` ao `.gitignore`
2. Manter um arquivo `.env.example` no repositório com as chaves mas sem valores sensíveis:

```
touch .env.example
```{{exec}}

```env
APP_ENV=
APP_PORT=
APP_NAME=
DB_HOST=
DB_PORT=
DB_NAME=
DB_USER=
DB_PASSWORD=
LOG_LEVEL=
```{{copy}}

Assim qualquer pessoa que clonar o projeto sabe quais variáveis precisa configurar, sem ter acesso aos valores reais.
