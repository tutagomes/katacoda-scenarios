## Parabéns!

Você agora sabe conectar uma API .NET ao SQL Server com persistência de dados garantida por volumes!

### Resumo dos conceitos

**Volumes nomeados no docker-compose.yml:**

```yaml
services:
  db:
    image: mcr.microsoft.com/mssql/server:2022-latest
    volumes:
      - dados-sqlserver:/var/opt/mssql   # nome-do-volume:caminho-no-container
    healthcheck:
      test: ["CMD-SHELL", "sqlcmd -S localhost -U sa -P ... -Q 'SELECT 1'"]
      interval: 10s
      retries: 10
      start_period: 30s

  api:
    depends_on:
      db:
        condition: service_healthy       # espera o banco estar saudável
    restart: on-failure                  # reinicia se falhar na inicialização

volumes:
  dados-sqlserver:                       # declaração do volume nomeado
```

**Comandos de volumes:**

```bash
docker volume ls                          # Lista todos os volumes
docker volume inspect <nome>              # Detalhes e localização no host
docker volume rm <nome>                   # Remove um volume
docker volume prune                       # Remove volumes não utilizados
docker compose down -v                    # Remove containers E volumes do projeto
```

**Regra prática:**

| Preciso de... | Use |
|---|---|
| Dados persistentes (banco, uploads) | Volume nomeado |
| Código compartilhado em dev | Bind mount (`./codigo:/app`) |
| Reset completo do ambiente | `docker compose down -v` |

### Próximo passo

Continue no cenário **"Docker Compose: Dev vs Produção"** para aprender a configurar ambientes diferentes com o mesmo `docker-compose.yml`!
