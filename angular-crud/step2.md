Agora, com tudo instalado e configurado, podemos começar a executar alguns comandos para criar nossa aplicação Angular!

O que o comando `ng` pode fazer?
`ng generate --help`{{execute "T1"}}

Bom, vamos começar então criando um projeto com o nome `angular-crud`

`ng new angular-crud`{{execute "T1"}}

E selecionar as opções

```
Stricter Type Checking - Y
Angular Router - Y
CSS
```

Agora, o CLI deve executar todos os comandos necessários para instalar as dependências do projeto além de criar para nós todos os componentes base

Então, navegamos até a pasta

`cd angular-crud`{{execute "T1"}}

E finalmente podemos ver a cara da aplicação básica:

`ng serve --host 0.0.0.0 --disable-host-check`{{execute "T1"}}

E acessá-la através da URL!
https://[[HOST_SUBDOMAIN]]-4200-[[KATACODA_HOST]].environments.katacoda.com

