Pronto, agora com nosso backend funcionando, podemos voltar ao que interessa!

Vamos começar criando nossos componentes Angular, sendo cada um respnsável por uma funcionalidade no nosso módulo de Clientes

Criamos então nosso módulo completo
`ng generate module clientes --routing`{{execute "T1"}}

E logo em seguida adicionamos todos os componentes que iremos utilizar
`ng generate component clientes/home`{{execute "T1"}}
`ng generate component clientes/detail`{{execute "T1"}}
`ng generate component clientes/create`{{execute "T1"}}
`ng generate component clientes/update`{{execute "T1"}}

Não podemos também nos esquecer do serviço responsável por comunicar com as nossas APIs
`ng generate service clientes/cliente`{{execute "T1"}}

E a interface para definirmos os campos do cliente!
`ng generate interface  clientes/cliente`{{execute "T1"}}

E aí sim, podemos executar novamente nossa aplicação!
`ng serve --host 0.0.0.0 --disable-host-check`{{execute "T1"}}

E acessá-la através da URL
https://[[HOST_SUBDOMAIN]]-4200-[[KATACODA_HOST]].environments.katacoda.com
