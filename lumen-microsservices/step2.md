Agora, vamos dar uma olhada no código fonte do arquivo `docker-compose.yml` localizado na pasta atual:
```
version: '3'
services:
  avaliacoes:
    build:
      dockerfile: Avaliacoes.Dockerfile
      context: ./micro_avaliacoes
    environment:
      USER_API: 'http://auth'
      LUGAR_API: 'http://lugares'
      REDIS_CLIENT: 'predis'
      REDIS_HOST: 'redis'

  auth:
    build:
      dockerfile: Auth.Dockerfile
      context: ./micro_auth

  gateway:
    build:
      dockerfile: Gateway.Dockerfile
      context: ./micro_gateway
    environment:
      USER_API: 'http://auth'
      LUGAR_API: 'http://lugares'
      REDIS_CLIENT: 'predis'
      REDIS_HOST: 'redis'
      AVALIACAO_API: 'http://avaliacoes'
    ports:
      - 9000:9000

  redis:
    image: redis
    command: redis-server
  
  lugares:
    build:
      dockerfile: Lugares.Dockerfile
      context: ./micro_lugares
    environment:
      USER_API: 'http://auth'
      LUGAR_API: 'http://lugares'
      REDIS_CLIENT: 'predis'
      REDIS_HOST: 'redis'
    ```

Note que ele contém informações sobre todos nossos serviços! Incluindo como fazer a build de cada um deles!
Para já começar com a build, vamos executar o comando:
`docker-compose build`{{execute}}

E quando a build terminar, podemos ir para o próximo passo