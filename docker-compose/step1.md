Vamos começar baixando o código exemplo de um pequeno servidor NodeJS com a configuração já feita, incluindo o NGinx (nosso balanceador).

Este repositório está pronto para ser testado, por isso já existe nele a pasta Testes. Mas por enquanto, vamos só executar a aplicação mesmo
`git clone https://github.com/tutagomes/servidor-confiabilidade.git`{{execute}}

Navegando até a pasta do Servidor:

`cd servidor-confiabilidade/Servidor`{{execute}}

Vamos primeiro criar a imagem do nosso servidor principal, hospedado em uma imagem NodeJS, bem simples e rápido! Ela será responsável por controlar toda a nossa aplicação. Criamos então com o comando:

`docker build -t teste-calculo-node .`

Caso queira aprofundar um pouco no código, pode-se perceber que a aplicação faz o uso de um banco de dados SQLite, salvo em disco. Portanto, os dados não serão sincronizados entre as instâncias executadas.
Se a sincronia de dados for necessária, é possível utilizar um banco de dados externo ou então montar um arquivo sqlite único para todos os contêineres.
