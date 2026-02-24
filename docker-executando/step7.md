## Monitorando e limpando o ambiente

### Monitorando recursos com docker stats

O `docker stats` exibe o consumo de CPU, memória, rede e disco de containers em execução em tempo real — como um `htop` específico para containers:

`docker stats nginx-explorer`{{execute}}

Pressione `Ctrl+C` para sair. Para uma leitura única sem atualização contínua:

`docker stats --no-stream nginx-explorer`{{execute}}

### Inspecionando detalhes com docker inspect

O `docker inspect` retorna todas as informações de configuração de um container em formato JSON — endereço IP, variáveis de ambiente, volumes montados, configurações de rede e muito mais:

`docker inspect nginx-explorer`{{execute}}

Para extrair apenas o endereço IP interno do container:

`docker inspect nginx-explorer -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'`{{execute}}

### Verificando o uso de disco

Com o tempo, imagens não utilizadas, containers parados e volumes órfãos acumulam e ocupam espaço. Comece verificando o que está sendo usado:

`docker system df`{{execute}}

O resultado mostra imagens, containers e volumes — e quanto espaço cada um ocupa e pode ser recuperado.

### Limpando o ambiente

Para remover recursos específicos:

```bash
docker container prune   # Remove todos os containers parados
docker image prune       # Remove imagens sem tag (dangling)
docker image prune -a    # Remove todas as imagens não usadas por nenhum container
docker volume prune      # Remove volumes sem containers associados
docker network prune     # Remove redes sem containers conectados
```

Para limpar tudo de uma vez (containers parados, redes e imagens sem uso):

`docker system prune`{{execute}}

> A flag `-a` inclui também imagens sem uso ativo: `docker system prune -a`. Use com atenção — será necessário baixar novamente tudo que for removido.

### Encerrando os containers deste cenário

`docker rm -f nginx-explorer nginx-web nginx-web2`{{execute}}

`docker ps -a`{{execute}}
