Vamos então criar uma nova imagem com nosso conteúdo.

Para isso, vamos começar criando uma imagem utilizando a versão do [Nginx Bitnami](https://hub.docker.com/r/bitnami/nginx/) para hospedar nossa aplicação de tarefas.

Como a aplicação já está pronta, vamos navegar até a pasta:

`cd website`{{execute}}

E listar o conteúdo

`ls -l`{{execute}}

Vamos também identificar o que há dentro do Dockerfile

`cat Dockerfile`

E por último, vamos executar a build da imagem:

`docker build -t cenario/tasks:latest .`{{execute}}