## Gerenciando serviços individualmente

Em uma aplicação com múltiplos serviços, às vezes você precisa agir em apenas um deles sem afetar os outros.

### Logs por serviço

`docker compose logs api`{{execute}}

`docker compose logs cache`{{execute}}

`docker compose logs -f api`{{execute}}

Pressione `Ctrl+C` para sair.

### Reiniciando apenas um serviço

Útil quando você quer reiniciar a API sem derrubar o Redis (e perder os dados em memória):

`docker compose restart api`{{execute}}

`docker compose ps`{{execute}}

### Parando e subindo um serviço específico

`docker compose stop api`{{execute}}

`docker compose ps`{{execute}}

O Redis continua rodando. Acesse o browser e veja que a API está indisponível enquanto o Redis permanece de pé.

`docker compose start api`{{execute}}

### Reconstruindo apenas um serviço

Quando você altera o código da API, não precisa reconstruir o Redis:

`docker compose up -d --build api`{{execute}}

### Executando um comando em um serviço específico

`docker compose exec cache redis-cli ping`{{execute}}

`docker compose exec cache redis-cli get visitas`{{execute}}

### Limpando o ambiente

`docker compose down`{{execute}}

> **Dica:** `docker compose down` remove os containers e a rede, mas os dados do Redis já se perdem quando o container para — pois o Redis guarda tudo em memória. No próximo cenário vamos resolver isso com volumes persistentes.
