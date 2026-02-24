## O que é Docker Compose

O Docker Compose é uma ferramenta que permite **definir e executar aplicações Docker** usando um arquivo YAML. Em vez de executar múltiplos comandos `docker run`, você descreve toda a sua aplicação em um único lugar.

### A estrutura do docker-compose.yml

```yaml
services:           # lista de containers da aplicação
  nome-do-servico:
    image: nginx    # imagem pronta do Hub
    # ou
    build: .        # constrói a partir de um Dockerfile local
    ports:
      - "8080:80"   # HOST:CONTAINER
    environment:
      - VARIAVEL=valor
    volumes:
      - ./local:/container
    depends_on:
      - outro-servico

volumes:            # volumes nomeados (opcional)
  meu-volume:

networks:           # redes customizadas (opcional)
  minha-rede:
```

### Serviço vs Container

No Compose, um **serviço** é a definição de como um container deve ser executado. Você pode inclusive escalar um serviço para rodar múltiplas instâncias do mesmo container.

### docker compose vs docker-compose

A partir do Docker Compose v2, o comando passou a ser integrado ao Docker:

```bash
docker compose up    # ✅ v2 — integrado ao Docker CLI (atual)
docker-compose up    # ⚠️  v1 — instalação separada (legado)
```

Neste cenário usaremos sempre `docker compose` (sem hífen).

### Verificando a instalação

`docker compose version`{{execute}}
