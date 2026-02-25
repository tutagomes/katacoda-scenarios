## Explorando o container por dentro

Uma das funcionalidades mais úteis do Docker é executar comandos dentro de um container em execução — sem precisar pará-lo ou recriá-lo. Isso é essencial para depurar problemas no dia a dia.

Vamos subir um container com porta mapeada para usar nos próximos passos:

`docker run --name nginx-explorer -d -p 8082:80 nginx`{{execute}}

### Executando um comando pontual

O `docker exec` executa qualquer comando dentro de um container que já está rodando:

`docker exec nginx-explorer ls /etc/nginx`{{execute}}

`docker exec nginx-explorer nginx -v`{{execute}}

### Abrindo um terminal interativo

Com as flags `-it`, conseguimos uma sessão interativa dentro do container:

- `-i` (*interactive*): mantém o canal de entrada aberto
- `-t` (*tty*): aloca um pseudo-terminal

`docker exec -it nginx-explorer /bin/bash`{{execute}}

Agora você está **dentro** do container. Explore:

`ls /`{{execute}}

`cat /etc/os-release`{{execute}}

`ls /usr/share/nginx/html/`{{execute}}

Perceba que o container tem seu próprio sistema de arquivos, completamente isolado da sua máquina, mas muito mais leve do que uma VM tradicional.

Para sair e voltar ao terminal da sua máquina:

`exit`{{execute}}

### Visualizando logs

O `docker logs` mostra tudo que o container escreveu para stdout/stderr — fundamental para depurar aplicações:

`curl -s localhost:8082 > /dev/null`{{execute}}

`docker logs nginx-explorer`{{execute}}

Para acompanhar os logs ao vivo (equivalente ao `tail -f`):

`docker logs -f nginx-explorer`{{execute}}

Acesse pelo browser: [Abrir na porta 8082]({{TRAFFIC_HOST1_8080}}) para visualizar a entrada nos logs.


Pressione `Ctrl+C` para parar de acompanhar. O container continua rodando normalmente.
