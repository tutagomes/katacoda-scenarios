## Arquivo de produção

Para produção, criamos um arquivo separado com as configurações do ambiente. Diferente do override, ele **não é carregado automaticamente** — precisa ser passado explicitamente com `-f`.

<pre class="file" data-filename="docker-compose.prod.yml" data-target="replace">
services:
  api:
    container_name: tarefas-api
    image: minha-org/tarefas-api:latest    # usa imagem publicada, não build local
    environment:
      - ASPNETCORE_ENVIRONMENT=Production
      - ConnectionStrings__Default=Server=db;Database=TarefasDb;User=sa;Password=${SA_PASSWORD};TrustServerCertificate=True
    restart: always
    deploy:
      resources:
        limits:
          memory: 512m
          cpus: "0.5"

  db:
    container_name: tarefas-db
    environment:
      - SA_PASSWORD=${SA_PASSWORD}         # lido do ambiente do host ou .env
    restart: always
    deploy:
      resources:
        limits:
          memory: 1g
          cpus: "1.0"
</pre>

### O que muda em produção

**`image:` em vez de `build:`** — em produção usamos uma imagem já construída e publicada no registry, não fazemos build na hora.

**`${SA_PASSWORD}`** — a senha vem de uma variável de ambiente do sistema ou de um secrets manager. Nunca hardcoded.

**`restart: always`** — o container reinicia automaticamente se cair.

**`deploy.resources.limits`** — define limites de CPU e memória para evitar que um serviço consuma recursos de outros.

**Sem `ports` no banco** — em produção o SQL Server não é exposto diretamente. Apenas a API é acessível externamente.

### Criando o arquivo .env para produção

Em produção, as variáveis sensíveis ficam em um arquivo `.env` que **não vai para o repositório**:

`echo "SA_PASSWORD=ProdSenha@Muito_Segura_2024!" > .env.prod`{{execute}}

> Em ambientes reais, use um secrets manager (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault) em vez de arquivos `.env`.
