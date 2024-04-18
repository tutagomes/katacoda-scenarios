## Comandos e solução para Múltiplas Versões

Caso tenha tido alguma dificuldade no passo anterior, aqui estão elencados em ordem de execução todos os comandos. Para isso, vamos assumir os seguintes pontos:

- O primeiro comando apagará a pasta app antes de começar para não haver conflito com uma pasta anterior
- Tenha certeza de que o terminal está na máquina raiz, não dentro de um contêiner (isso pode ser validado na linha do terminal, que deve dizer `ubuntu $`).
---

Pronto, agora podemos começar:

1. Apagando a pasta app anterior:

   `rm -rf app`{{execute}}

1. Criando uma pasta `app`

    `mkdir app`{{execute}}
1. Executando o SDK 7:

   `docker run --rm -v ./app:/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:7.0`{{execute}}
1. Verificando a versão do `dotnet` (deve ser a 7)

   `dotnet --version`{{execute}}

1. Navegando até a pasta `/app`

    `cd /app`{{execute}}

1. Executando o comando para criar uma aplicação

   `dotnet new webapi`{{execute}}

1. Executando a aplicação 

    `dotnet run --urls=http://0.0.0.0:5000`{{execute}}
1. Validando o acesso:
    
    Porta 5000: [Acesso ao WebAPI]({{TRAFFIC_HOST1_5000}}/WeatherForecast)
    
    Se estiver tudo certo, pode parar com o comando CTRL+C

1. Saindo do contêiner
   
   `exit`{{execute}}
---
1. Executando o SDK 6:

   `docker run --rm -v ./app:/app --workdir=/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:6.0`{{execute}}
1. Verificando a versão do `dotnet` (deve ser a 6)

   `dotnet --version`{{execute}}


1. Executando a aplicação 

    `dotnet run --urls=http://0.0.0.0:5000`{{execute}}

1. Saindo do contêiner
   
   `exit`{{execute}}

---
1. Executando o SDK 8:

   `docker run --rm -v ./app:/app --workdir=/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:8.0`{{execute}}
1. Verificando a versão do `dotnet` (deve ser a 6)

   `dotnet --version`{{execute}}

1. Executando a aplicação 

    `dotnet run --urls=http://0.0.0.0:5000`{{execute}}

1. Validando o acesso:
    
    Porta 5000: [Acesso ao WebAPI]({{TRAFFIC_HOST1_5000}}/WeatherForecast)
    
    Se estiver tudo certo, pode parar com o comando CTRL+C

1. Saindo do contêiner
   
   `exit`{{execute}}

*Não esqueça de alterar o TargetFramework do arquivo `app.csproj` para `8.0`!*

