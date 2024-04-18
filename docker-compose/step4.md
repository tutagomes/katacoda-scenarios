## Comandos e solução para Múltiplos Estágios

Caso tenha tido alguma dificuldade no passo anterior, aqui estão elencados em ordem de execução todos os comandos. Para isso, vamos assumir os seguintes pontos:

- Já estamos na pasta `app` e já existe um arquivo `Dockerfile` criado.
- Tenha certeza de que o terminal está na máquina raiz, não dentro de um contêiner (isso pode ser validado na linha do terminal, que deve dizer `ubuntu $`).

---

Pronto, agora podemos começar:

O Dockerfile correto é: 
```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 5000
EXPOSE 8080

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["app.csproj", "."]
RUN dotnet restore "./app.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "app.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "app.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "app.dll"]
```{{copy}}

E o comando para construir a imagem:

`docker build -t cenario/api-teste:latest .`{{execute}}


E o comando para rodar a imagem:

`docker run -p 8080:8080 --rm cenario/api-teste:latest`{{execute}}

Porta 8080: [Acesso ao WebAPI]({{TRAFFIC_HOST1_8080}}/WeatherForecast)

Para sair, vamos apertar CTRL+C.

