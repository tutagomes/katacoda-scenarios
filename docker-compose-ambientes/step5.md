## Profiles para serviços opcionais

Às vezes você tem serviços que só devem subir em contextos específicos — um painel de administração do banco em dev, uma ferramenta de métricas apenas quando debugando, um serviço de e-mail fake em testes.

O `profiles` permite marcar serviços como opcionais e ativá-los explicitamente quando necessário.

### Adicionando um serviço com profile

Vamos adicionar o **Adminer** (interface web para o SQL Server) que só deve subir em dev quando necessário:

<pre class="file" data-filename="docker-compose.override.yml" data-target="replace">
services:
  api:
    container_name: tarefas-api-dev
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__Default=Server=db;Database=TarefasDb;User=sa;Password=DevSenha@123;TrustServerCertificate=True

  db:
    container_name: tarefas-db-dev
    environment:
      - SA_PASSWORD=DevSenha@123
    ports:
      - "1433:1433"

  adminer:
    image: adminer
    container_name: tarefas-adminer
    ports:
      - "8081:8080"
    profiles:
      - ferramentas
    networks:
      - rede-app
</pre>

### Subindo sem o serviço opcional

`docker compose up -d --build`{{execute}}

`docker compose ps`{{execute}}

O Adminer não subiu — serviços com `profiles` só sobem se explicitamente ativados.

### Subindo com o serviço opcional

`docker compose --profile ferramentas up -d`{{execute}}

`docker compose ps`{{execute}}

Agora o Adminer está rodando. Acesse:

[Abrir Adminer na porta 8081]({{TRAFFIC_HOST1_8081}})

Use os dados: servidor `db`, usuário `sa`, senha `DevSenha@123`.

### Múltiplos profiles

Um serviço pode pertencer a múltiplos profiles, e você pode ativar mais de um ao mesmo tempo:

```yaml
servico-debug:
  profiles:
    - ferramentas
    - debug

# Ativa os dois profiles:
# docker compose --profile ferramentas --profile debug up -d
```

### Limpando o ambiente

`docker compose --profile ferramentas down`{{execute}}
