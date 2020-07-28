Com uma imagem disponível, agora nós podemos disparar um conteiner. Para isso rode o seguinte comando:
`docker run -d --name nginx1 nginx`{{execute}}

Após você disparar o seu conteiner, você pode examinar os conteineres que foram iniciados com o seguinte comando:

`docker ps`{{execute}}

Observe que esse conteiner Web não teve a sua porta 80 exposta para o sistema operacional e portanto não é acessível. Para expor a porta 80 precisamos rodar o nosso conteiner da seguinte forma:

`docker run --name nginx2 -d -p 80:80 nginx`{{execute}}

Examine que temos agora dois conteineres disparados. Você pode examinar isso com o comando ps

`docker ps`{{execute}}

Observe que o ultimo conteiner tem a sua porta 80 exposta. Podemos acessar esse servidor com o comando abaixo.

`curl docker`{{execute}}

Vamos agora disparar mais um conteiner nginx. Se usássemos o comando docker run -d -p 80:80 nginx teríamos um erro pois a porta 80 do sistema operacional nativo já está ocupada.

Então vamos expor um novo conteiner na porta 81. Para isso use o seguinte comando:

`docker run --name nginx3 -d -p 81:80 nginx`{{execute}}

Podemos examinar que temos agora três conteineres operando. O primeiro não está expondo nenhuma porta, enquanto os dois últimos estão expondo o servidor Web nas portas 80 e 81, respectivamente.

`docker ps`{{execute}}

`curl docker:81`{{execute}}

O acesso externo está disponível nos links:
Porta 80: https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/
Porta 81: https://[[HOST_SUBDOMAIN]]-81-[[KATACODA_HOST]].environments.katacoda.com/

