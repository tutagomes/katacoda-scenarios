E também podemos entrar no contâiner e identificar os arquivos que estão lá dentro!

`docker exec -ti myappwithvolume /bin/bash`{{execute}}

`cd /app`{{execute}}

`ls -l`{{execute}}

Agora devemos ver a lista dos arquivos que criamos anteriormente, como Dockerfile, index.html e outro.html.

Assim como uma máquina virtual, o contêiner possui seu próprio Sistema Operacional e seus próprios arquivos!

Se tivéssemos permissões suficientes, poderíamos também atualizar o sistema, instalar novos pacotes ou até mesmo utilizar como uma máquina virtual.

`apt update`{{execute}}

`apt upgrade`{{execute}}

`date`{{execute}}