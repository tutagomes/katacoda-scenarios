## Explorando o Container

Uma das funcionalidades mais poderosas do Docker é a capacidade de "entrar" em um container em execução para inspecionar o que está acontecendo lá dentro. Isso é extremamente útil para depurar problemas.

Primeiro, vamos subir um container novo:

`docker run --name nginx-explorer -d -p 8082:80 nginx`{{execute}}

### Executando um comando dentro do container

O comando `docker exec` permite rodar qualquer comando dentro de um container que já está em execução, sem precisar entrar nele:

`docker exec nginx-explorer ls /etc/nginx`{{execute}}

### Abrindo um terminal interativo

Com as flags `-it`, conseguimos uma sessão interativa — como se estivéssemos abrindo um terminal diretamente dentro do container:

- `-i` (*interactive*): mantém o canal de entrada aberto
- `-t` (*tty*): aloca um pseudo-terminal para interação

`docker exec -it nginx-explorer /bin/bash`{{execute}}

Agora você está **dentro** do container. Explore um pouco:

`ls /`{{execute}}

`nginx -v`{{execute}}

`cat /etc/os-release`{{execute}}

Perceba que o container tem seu próprio sistema de arquivos e seu próprio sistema operacional — completamente isolado da sua máquina, mas muito mais leve do que uma VM.

Para sair do container e voltar ao terminal da sua máquina:

`exit`{{execute}}

### Copiando arquivos com docker cp

O `docker cp` copia arquivos entre o host e um container em execução — sem precisar montar um volume. É muito útil para extrair logs, configs ou injetar um arquivo pontualmente.

```bash
# Do host para o container
docker cp arquivo.txt nginx-explorer:/tmp/arquivo.txt

# Do container para o host
docker cp nginx-explorer:/etc/nginx/nginx.conf ./nginx.conf
```

Vamos extrair o arquivo de configuração do nginx:

`docker cp nginx-explorer:/etc/nginx/nginx.conf ./nginx-conf-extraido.conf`{{execute}}

`ls -la nginx-conf-extraido.conf`{{execute}}

### Monitorando recursos em tempo real

O `docker stats` exibe o consumo de CPU, memória, rede e disco de containers em execução — em tempo real, como um `htop` para containers:

`docker stats nginx-explorer`{{execute}}

Pressione `Ctrl+C` para sair. Para uma leitura única sem ficar atualizando:

`docker stats --no-stream nginx-explorer`{{execute}}

### Limpando o ambiente

Com o tempo, imagens não utilizadas, containers parados e volumes órfãos acumulam e ocupam espaço em disco. O Docker oferece comandos de limpeza para cada tipo:

`docker system df`{{execute}}

O `docker system df` mostra quanto espaço está sendo ocupado por imagens, containers e volumes — e quanto pode ser recuperado.

Para limpar seletivamente:

```bash
docker image prune       # Remove imagens sem tag (dangling)
docker image prune -a    # Remove todas as imagens não usadas por algum container
docker container prune   # Remove todos os containers parados
docker volume prune      # Remove volumes sem containers associados
docker network prune     # Remove redes sem containers conectados
```

Para limpar tudo de uma vez:

`docker system prune`{{execute}}

> A flag `-a` inclui também imagens sem uso: `docker system prune -a`. Use com cuidado — você precisará baixar novamente tudo que for removido.

### Inspecionando detalhes do container

O comando `docker inspect` retorna todas as informações de configuração de um container em formato JSON — endereço IP, variáveis de ambiente, volumes montados, configurações de rede e muito mais:

`docker inspect nginx-explorer`{{execute}}

Para encontrar o endereço IP interno do container:

`docker inspect nginx-explorer | grep IPAddress`{{execute}}
