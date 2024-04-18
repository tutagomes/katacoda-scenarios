## Múltiplas versões do .NET Core

Este passo explora a utilização de containers Docker para gerenciar múltiplas versões do .NET Core simultaneamente, o que é crucial em ambientes onde diferentes versões da plataforma precisam ser testadas ou usadas em paralelo. Docker facilita o gerenciamento dessas versões, permitindo que você crie, teste e execute aplicações sem necessitar instalar múltiplas SDKs na sua máquina. Isso não apenas economiza tempo, mas também reduz conflitos entre versões e simplifica a configuração de ambientes de desenvolvimento e testes.

As imagens .NET Core estão dividas em:
- SDK (contendo todas as bibliotecas para compilação do framework/projeto)
- ASP.NET (com todas as dependências para aplicações web)
- Runtime (com todas as dependências para aplicações .NET padrão)

Assim, temos uma imagem preparada para cada objetivo que precisarmos, reduzindo o tamanho final e os passos de compilação. Normalmente, quando estamos programando em uma versão do .NET e precisamos atualizá-la, é preciso instalar os pacotes no nosso computador em paralelo. Dessa forma, não é possível testar de forma isolada se a aplicação que estamos validando funciona nas versões mais recentes (ou antigas) do framework.

**E é isso que vamos testar hoje!**

---
### Passo 1:

Vamos primeiro criar uma pasta local, podemos chamá-la de `app`.

Depois, vamos executar um contêiner docker na versão do `SDK 7 do dotnet` de forma interativa
  
  `docker run --rm -v ./app:/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:7.0`{{execute}}

As versões das imagens são:
- SDK 6 - mcr.microsoft.com/dotnet/sdk:6.0
- SDK 7 - mcr.microsoft.com/dotnet/sdk:7.0
- SDK 8 - mcr.microsoft.com/dotnet/sdk:8.0

E vamos utilizar os parâmetros `-v ./app:/app`, `-it`, `-p 5000:5000`, `--entrypoint=/bin/bash` para exeuctar nosso contêiner com dotnet.

### Passo 2

Após subir o contêiner mapeando a pasta, vamos navegar até a pasta `/app` e criar uma aplicação nova com o comando `dotnet new webapi`.

Em seguida, vamos executar o comando `dotnet run --urls=http://0.0.0.0:5000` para rodar nossa aplicação.

Porta 5000: [click here]({{TRAFFIC_HOST1_5000}}/WeatherForecast)

Após a criação, vamos validar se os arquivos estão presentes através do nosso navegador a direita.

Vamos sair do terminal com o comando `exit` e você pode perceber que voltamos para o terminal do ambiente (não mais dentro do container).

### Passo 3:

Agora, vamos executar o mesmo comando para subir o container, mas dessa vez com a imagem do sdk 6, navegar até a pasta `/app` e tentar executar o comando `dotnet run`. O que acontece?

### Passo 4:

Agora, vamos executar o mesmo comando para subir o container com SDK 8, navegando até o diretório `/app` e executando o comando `dotnet run --urls=http://0.0.0.0:5000`. O que acontece?

Porta 5000: [click here]({{TRAFFIC_HOST1_5000}}/WeatherForecast)

*Só avance quando terminar a execução!*