## Parabéns!

Você agora domina o gerenciamento de ambientes com Docker Compose — uma habilidade essencial para qualquer equipe que trabalha com containers em produção!

### Resumo dos padrões aprendidos

**Estrutura de arquivos recomendada:**

```
docker-compose.yml          # base — comum a todos os ambientes
docker-compose.override.yml # dev — carregado automaticamente
docker-compose.prod.yml     # produção — passado com -f
.env.prod                   # variáveis sensíveis de produção (no .gitignore)
.env.example                # template das variáveis (no repositório)
```

**Comandos por ambiente:**

```bash
# Desenvolvimento (automático)
docker compose up -d --build

# Produção (explícito)
docker compose -f docker-compose.yml -f docker-compose.prod.yml --env-file .env.prod up -d

# Ver configuração mesclada (antes de aplicar)
docker compose config
docker compose -f docker-compose.yml -f docker-compose.prod.yml config
```

**Profiles:**

```bash
# Sobe serviços padrão
docker compose up -d

# Sobe serviços padrão + serviços do profile "ferramentas"
docker compose --profile ferramentas up -d

# Derruba tudo incluindo serviços com profile
docker compose --profile ferramentas down
```

### A trilha completa

Você concluiu a trilha de Docker Compose! Aqui está o caminho percorrido:

```
Introdução          → Sintaxe, comandos essenciais
Múltiplos Serviços  → Comunicação, depends_on, healthcheck
Banco e Persistência→ Volumes nomeados, SQL Server + EF Core
Dev vs Produção     → Override, -f, profiles, secrets
```

Com isso, você tem o conhecimento necessário para colocar aplicações .NET reais em containers de forma profissional!
