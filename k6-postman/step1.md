Primeiro, vamos clonar o repositório e validar o conteúdo:

`git clone https://github.com/tutagomes/teste-carga-k6-postman.git`{{execute}}

E também, vamos preparar nossa imagem base para uso. Ela consiste em uma API simples (CRUD) com uma implementação em duas versões.

Você pode perceber que, dentro do nosso repositório, existem dois arquivos:
- Postman-Teste-V1.json
- Postman.Teste-V2.json

Cada um deles é responsável por apontar e testar uma versão específica da nossa API, que está codificada na pasta `src`. Não se preocupe com o código, estamos executando um teste de caixa preta, nos interessando somente nos resultados.

Antes de ir para o próximo passo, vamos já deixar nossa imagem construída com o comando:

`cd teste-carga-k6-postman`{{execute}}

`docker-compose build`{{execute}}

E também, iremos precisar do Node instalado, mas como estamos num Ubuntu, basta executar:

`sudo apt install nodejs -y`{{execute}}

E claro, nosso K6:

`sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C5AD17C747E3415A3642D57D77C6C491D6AC1D69`{{execute}}


`echo "deb https://dl.k6.io/deb stable main" | sudo tee /etc/apt/sources.list.d/k6.list`{{execute}}


`sudo apt-get update`{{execute}}


`sudo apt-get install k6`{{execute}}