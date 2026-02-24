## Docker Compose

Nos cenários anteriores você aprendeu a rodar containers com `docker run`. Funciona bem para um container — mas imagine ter que digitar isso toda vez:

```bash
docker run -d --name api \
  --network minha-rede \
  -p 8080:8080 \
  -e ASPNETCORE_ENVIRONMENT=Development \
  -e DB_HOST=db \
  -e REDIS_HOST=cache \
  minha-api:latest
```

E isso é só para **um** serviço. Uma aplicação real pode ter cinco ou dez.

O Docker Compose resolve isso com um único arquivo YAML que descreve todos os serviços, redes e volumes da sua aplicação. Em vez de comandos longos, você escreve uma vez e executa com:

```bash
docker compose up
```

### O que você vai aprender neste cenário

- A estrutura do arquivo `docker-compose.yml`
- Como subir uma API .NET com Docker Compose
- Os comandos essenciais: `up`, `down`, `ps`, `logs`
- Como passar variáveis de ambiente pelo Compose
- Como reconstruir a imagem após mudanças no código

> **Pré-requisito:** Este cenário assume que você já sabe criar Dockerfiles e entende o básico de `docker run`. Se ainda não sabe, comece pelos cenários **"Docker: Criando sua primeira imagem"** e **"Docker: Variáveis de Ambiente"**.
