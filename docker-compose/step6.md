## Comandos e solução para Docker Compose

Caso tenha tido alguma dificuldade no passo anterior, aqui estão elencados em ordem de execução todos os comandos. Para isso, vamos assumir os seguintes pontos:

- O arquivo docker-compose já está criado, vamos apenas alterá-lo
- Os arquivos do projeto já estão alterados
- Estamos na pasta `app`

---

Pronto, agora podemos começar:

1. O docker-compose.yaml deve ser:

```yaml
version: '3.3'
services:
  webapi:
    container_name: aplicacao
    image: cenario/api-teste:latest
    ports:
      - "8080:8080"
    depends_on:
      - db
    environment:
      - ConnectionStrings__DefaultConnection=Server=database;Database=master;User=sa;Password=senha_qualquer#_1;

  db:
    container_name: database
    image: "mcr.microsoft.com/mssql/server:2022-latest"
    environment:
      SA_PASSWORD: "senha_qualquer#_1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"
```

1. Executando o SDK 7:

   `docker run --rm -v ./app:/app --entrypoint=/bin/bash -t -i -p 5000:5000  mcr.microsoft.com/dotnet/sdk:7.0`{{execute}}

1. Adicionar as dependências:

   `dotnet add package Microsoft.EntityFrameworkCore.SqlServer`{{execute}}

1. Sair do container:

   `exit`{{execute}}

1. Compilar a imagem novamente:

    `docker build -t cenario/api-teste:latest .`{{execute}}

1. Executar o docker-compose

   `docker-compose up`{{execute}}


Porta 8080: [Acesso ao WebAPI de Livros]({{TRAFFIC_HOST1_8080}}/api/Livros)

*PS: nada deve acontecer, pois os dados ainda não estão no banco!*