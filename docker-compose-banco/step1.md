## O problema dos dados que somem

Antes de usar volumes, vamos demonstrar o problema que eles resolvem.

### Subindo um SQL Server sem volume

`docker run -d --name sql-teste \`{{execute}}
`  -e ACCEPT_EULA=Y \`{{execute}}
`  -e SA_PASSWORD=Senha@123 \`{{execute}}
`  mcr.microsoft.com/mssql/server:2022-latest`{{execute}}

Aguarde alguns segundos para o SQL Server inicializar:

`sleep 20`{{execute}}

Crie um banco e insira um dado:

`docker exec sql-teste /opt/mssql-tools18/bin/sqlcmd \`{{execute}}
`  -S localhost -U sa -P Senha@123 -No \`{{execute}}
`  -Q "CREATE DATABASE Teste; USE Teste; CREATE TABLE Itens (Id INT, Nome NVARCHAR(50)); INSERT INTO Itens VALUES (1, 'Item que vai sumir')"`{{execute}}

Verifique que o dado existe:

`docker exec sql-teste /opt/mssql-tools18/bin/sqlcmd \`{{execute}}
`  -S localhost -U sa -P Senha@123 -No \`{{execute}}
`  -Q "USE Teste; SELECT * FROM Itens"`{{execute}}

### Agora remova o container

`docker rm -f sql-teste`{{execute}}

### Suba novamente e tente acessar os dados

`docker run -d --name sql-teste \`{{execute}}
`  -e ACCEPT_EULA=Y \`{{execute}}
`  -e SA_PASSWORD=Senha@123 \`{{execute}}
`  mcr.microsoft.com/mssql/server:2022-latest`{{execute}}

`sleep 20`{{execute}}

`docker exec sql-teste /opt/mssql-tools18/bin/sqlcmd \`{{execute}}
`  -S localhost -U sa -P Senha@123 -No \`{{execute}}
`  -Q "USE Teste; SELECT * FROM Itens"`{{execute}}

O banco `Teste` não existe mais. **Todo o conteúdo foi perdido junto com o container.**

Limpe antes de continuar:

`docker rm -f sql-teste`{{execute}}

Nos próximos passos vamos criar a mesma estrutura com Docker Compose e volumes para que isso não aconteça.
