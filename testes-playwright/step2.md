Agora, vamos começar com a diversão.



Para que seja possível executar os testes com Playwright, é necessário que estejam encapsulados dentro de um projeto Node.

Vamos começar então criando um projeto vazio:

`npm init -y`{{execute}}



E instalando a dependência do framework como dependência de desenvolvimento, para que no futuro, ele não seja empacotado na versão de produção:
`npm i -D @playwright/test`{{execute}}



E finalmente, vamos instalar as dependências (os navegadores que vamos utilizar)
`npx playwright install`{{execute}}

E como estamos em um Ubuntu simples, vamos precisar também instalar algumas dependências do Playwright

`npx playwright install-deps`{{execute}}



Note que o comando instalará todos os navegadores disponíveis, mas poderemos alterar em qual deles nossos testes serão executados.

