## Parabéns!

Você agora sabe usar o Docker Compose para definir e gerenciar aplicações .NET em containers!

### Resumo dos comandos aprendidos

```bash
docker compose up              # Sobe os serviços (foreground)
docker compose up -d           # Sobe em background (detached)
docker compose up -d --build   # Reconstrói as imagens antes de subir
docker compose down            # Para e remove containers e redes
docker compose down -v         # Idem, removendo também os volumes
docker compose stop            # Para os containers (sem remover)
docker compose start           # Reinicia containers parados
docker compose ps              # Lista serviços e seus estados
docker compose logs            # Exibe logs de todos os serviços
docker compose logs -f         # Logs em tempo real
docker compose logs <servico>  # Logs de um serviço específico
docker compose exec <servico> <cmd>  # Executa comando em um serviço
docker compose build           # Reconstrói imagens sem subir containers
```

### Estrutura mínima do docker-compose.yml

```yaml
services:
  meu-servico:
    build: .                         # ou image: nginx
    container_name: meu-container
    ports:
      - "8080:8080"
    environment:
      - VARIAVEL=valor
```

### Próximo passo

Continue no cenário **"Docker Compose: Múltiplos Serviços"** para aprender a orquestrar uma API .NET junto com o Redis!
