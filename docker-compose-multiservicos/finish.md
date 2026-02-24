## Parabéns!

Você agora sabe orquestrar múltiplos serviços com Docker Compose — incluindo comunicação entre containers, ordem de inicialização segura e gerenciamento individual de serviços!

### Resumo dos conceitos aprendidos

**Múltiplos serviços:**
```yaml
services:
  api:
    build: .
    environment:
      - REDIS_HOST=cache     # usa o nome do serviço como hostname
    depends_on:
      cache:
        condition: service_healthy

  cache:
    image: redis:alpine
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      retries: 5
```

**Rede automática:** O Compose cria uma rede e todos os serviços se comunicam pelo nome do serviço — sem configuração manual de IPs.

**Comandos para serviços individuais:**
```bash
docker compose logs <servico>          # Logs de um serviço
docker compose restart <servico>       # Reinicia um serviço
docker compose stop/start <servico>    # Para/inicia um serviço
docker compose up -d --build <servico> # Reconstrói e reinicia um serviço
docker compose exec <servico> <cmd>    # Executa comando no serviço
```

### Próximo passo

Continue no cenário **"Docker Compose: Banco de Dados e Persistência"** para aprender a manter dados entre reinicializações com volumes!
