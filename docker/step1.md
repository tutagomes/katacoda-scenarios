O Docker como ferramenta, cria uma camada de abstação e isolamento para nossas aplicações. E os desenvolvedores também mantém um dos maiores repositórios de imagens públicas, sendo possível visualizar, baixar e até mesmo publicar suas próprias! Por padrão, ele está disponível através do site http://hub.docker.com e lá podemos analisar todas as imagens públicas já enviadas.

Para começar, vamos tentar criar um contâiner com Nginx, mas primeiro, vamos procurar esta imagem no Docker Hub:

`docker search nginx`{{execute}}

Note que é possível ver inúmeras variações das imagens que contém a palavra nginx, mas vamos escolher a oficial!

Para baixar é simples, basta executar o comando:

`docker pull nginx`{{execute}}

Assim que finalizar (o que deve ser bem rápido) podemos então listar as imagens que estão salvas no disco da máquina através do comando:

`docker images`{{execute}}