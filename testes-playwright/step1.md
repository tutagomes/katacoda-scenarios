

Verificar a versão do Node

```sh
    node -v
```

Verificar se os arquivos estão disponíveis
```
    ls
```

Deve ser possível visualizar o arquivo "index.html"

E vamos também subir um simples servidor para acessar o nosso index.html para testes

```
    docker run --name meu-website -v /root/website:/usr/share/nginx/html:ro -d -p 8080:80 nginx
```

E deve então estar disponível em:

Porta 8080: https://[[HOST_SUBDOMAIN]]-8080-[[KATACODA_HOST]].environments.katacoda.com/
