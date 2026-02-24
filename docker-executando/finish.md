## Parabéns!

Você concluiu o cenário e agora domina os comandos fundamentais para trabalhar com containers Docker!

### Resumo dos comandos aprendidos

**Imagens:**

```bash
docker search <termo>      # Busca imagens no Docker Hub
docker pull <imagem>       # Baixa uma imagem
docker images              # Lista imagens baixadas
docker rmi <imagem>        # Remove uma imagem
```

**Containers:**

```bash
docker run -d --name <nome> <imagem>        # Cria e inicia um container em background
docker run -d --name <nome> -p 8080:80 <imagem>  # Com mapeamento de porta
docker ps                                   # Lista containers em execução
docker ps -a                                # Lista todos os containers (incluindo parados)
docker stop <nome>                          # Para um container
docker start <nome>                         # Reinicia um container parado
docker rm <nome>                            # Remove um container
docker rm -f <nome>                         # Para e remove em um único comando
```

**Inspeção e cópia:**

```bash
docker exec <nome> <comando>        # Executa um comando dentro do container
docker exec -it <nome> /bin/bash    # Abre terminal interativo no container
docker inspect <nome>               # Exibe todos os detalhes do container
docker cp <nome>:<caminho> .        # Copia arquivo do container para o host
docker cp ./arquivo <nome>:<caminho># Copia arquivo do host para o container
docker stats <nome>                 # Monitora CPU/memória em tempo real
docker stats --no-stream <nome>     # Leitura única de recursos
```

**Limpeza:**

```bash
docker system df                    # Mostra uso de disco do Docker
docker system prune                 # Remove containers, redes e imagens não usados
docker system prune -a              # Idem, incluindo imagens sem uso
docker container prune              # Remove containers parados
docker image prune -a               # Remove imagens sem containers associados
docker volume prune                 # Remove volumes órfãos
```

### Próximo passo

Agora que você sabe executar containers, que tal aprender a **criar a sua própria imagem**?

Continue no cenário **"Docker: Criando sua primeira imagem"**!
