## Dockerfile de múltiplos estágios

No passo anterior, vimos como podemos executar várias versões do .NET sem precisar instalá-las, resultando em um processo mais consistente e limpo ao determinar as dependências necessárias. Agora vamos ver sobre a construção de imagens.

A construção de imagens Docker utilizando o método multi-stage é uma prática avançada que permite a otimização de imagens, reduzindo seu tamanho e melhorando a segurança e a eficiência do ambiente de execução. Este método consiste em usar múltiplos estágios de build dentro de um único Dockerfile, permitindo compilar e construir a aplicação em estágios separados e, finalmente, criar uma imagem final mais limpa e leve. Isto é particularmente útil para linguagens compiladas como o .NET Core, onde o ambiente de build pode ser significativamente maior que o ambiente necessário para executar a aplicação.

**Objetivo**: Criar um Dockerfile eficiente que utilize multi-stage build para compilar e executar uma Web API.NET Core, minimizando o tamanho final da imagem e isolando ambientes de build e de execução.

---

Primeiro, será necessário navegar até a pasta `app` no terminal.

Em seguida, podemos criar um arquivo de nome `Dockerfile` na raiz do diretório `app`.

Você pode usar de exemplo o arquivo, mas não se esqueça de alterar o nome dos arquivos e a versão do SDK e AspNET:

```dockerfile

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 5000
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["apidemo.csproj", "."]
RUN dotnet restore "./apidemo.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "apidemo.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "apidemo.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "apidemo.dll"]
```{{copy}}

E vamos executar o comando `docker build -t cenario/api-teste:latest .`

Após a build, vamos executar com o comando `docker run` lembrando de expor a porta 8080 para podermos acessá-la de fora da máquina local.


Porta 8080: [Acesso ao WebAPI]({{TRAFFIC_HOST1_8080}}/WeatherForecast)