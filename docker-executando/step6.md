## Copiando arquivos com docker cp

O `docker cp` copia arquivos entre o host e um container em execução — sem precisar montar volumes nem recriar o container. É muito útil para injetar arquivos pontualmente ou extrair logs e configurações.

```bash
# Do host para o container
docker cp arquivo.txt nome-container:/caminho/dentro/container

# Do container para o host
docker cp nome-container:/caminho/dentro/container ./arquivo.txt
```

### Copiando um arquivo para dentro do container

Vamos criar uma página HTML personalizada e substituir a página padrão do nginx:

`cat > index.html << 'EOF'
<!DOCTYPE html>
<html>
<head><title>Meu Nginx no Docker</title></head>
<body>
  <h1>Olá do Docker!</h1>
  <p>Este arquivo foi copiado para dentro do container com <code>docker cp</code>.</p>
</body>
</html>
EOF`{{execute}}

Agora copie o arquivo para o diretório de arquivos estáticos do nginx dentro do container:

`docker cp index.html nginx-explorer:/usr/share/nginx/html/index.html`{{execute}}

Confirme que funcionou — o nginx deve servir o novo conteúdo imediatamente, sem precisar reiniciar:

`curl localhost:8082`{{execute}}

Ou acesse pelo navegador: [Abrir na porta 8082]({{TRAFFIC_HOST1_8082}})

### Copiando um arquivo para fora do container

Também é possível extrair arquivos de dentro do container — útil para recuperar logs, configs ou qualquer outro arquivo gerado em tempo de execução:

`docker cp nginx-explorer:/etc/nginx/nginx.conf ./nginx-conf-extraido.conf`{{execute}}

`ls -lh nginx-conf-extraido.conf`{{execute}}

`cat nginx-conf-extraido.conf`{{execute}}

> O `docker cp` funciona mesmo com o container em execução e não interrompe nenhum processo. Para uso contínuo em produção, volumes são a abordagem recomendada — mas `docker cp` é perfeito para inspeções pontuais.
