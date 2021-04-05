Primeiro, vamos instalar todas as dependências que precisamos para executar corretamente nosso fluxo!


Vamos atualizar nossas referências de pacotes:

`sudo apt-get update`{{execute "T1"}}

E finalmente garantir que o NodeJS esteja instalado!

`sudo apt-get upgrade nodejs -y`{{execute "T1"}}

Assim, fica fácil instalar nossos pacotes globalmente, como o Angular/CLI e o Json-server!
 
`npm install -g @quasar/cli json-server`{{execute "T1"}}