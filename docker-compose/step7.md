## Populando o SQL

No passo anterior, subimos nossa WebApi e a correta comunicação com o banco de dados utilizando docker-compose. No entanto, não conseguimos recuperar nenhuma informação, pois o banco estava vazio! Agora vamos ver como resolver isso de forma automatizada.

Incorporar dados iniciais em um banco de dados SQL Server que está sendo hospedado em um container Docker é uma prática comum para preparar ambientes de desenvolvimento, testes, ou mesmo produção, com um conjunto de dados padrão. Isso pode ser especialmente útil para aplicações que dependem de dados específicos para funcionar corretamente desde o início.

**Objetivo**: Configurar um container SQL Server para iniciar com um script SQL que cria e popula a tabela Livros com dados iniciais, garantindo que a aplicação Web API possa interagir com esses dados logo após a subida dos serviços.

---

Vamos criar um arquivo `init.sql` com o seguinte conteúdo:

```sql
IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Livros')
BEGIN
    CREATE TABLE Livros (
        Id INT PRIMARY KEY IDENTITY,
        Nome NVARCHAR(100),
        Autor NVARCHAR(100),
        NumeroPaginas INT,
        ISBN NVARCHAR(20) UNIQUE,
        QuantidadeVendas INT,
        Avaliacao FLOAT
    );
END

-- Inserção de dados idempotente, verifica se a tabela está vazia
IF NOT EXISTS (SELECT * FROM Livros)
BEGIN
    INSERT INTO Livros (Nome, Autor, NumeroPaginas, ISBN, QuantidadeVendas, Avaliacao)
    VALUES 
        ('A Saga do Código', 'Cecília Borges', 320, '978-4567890123', 85, 4.7),
        ('Jornadas JavaScript', 'Augusto Lima', 215, '978-4567890124', 120, 4.3),
        ('Python pelo Caminho', 'Renato Dias', 198, '978-4567890125', 150, 4.9),
        ('Contos de C#', 'Luciana Martins', 354, '978-4567890126', 95, 3.8),
        ('Aventuras em Go', 'Marcos Ribeiro', 289, '978-4567890127', 110, 4.2),
        ('Rust em Ação', 'Patrícia Queiroz', 234, '978-4567890128', 90, 4.5),
        ('Swift e o Segredo das Apps', 'Rodrigo Souza', 310, '978-4567890129', 80, 4.0),
        ('Os Segredos do Node', 'Tânia Alves', 303, '978-4567890130', 140, 4.6),
        ('Peripécias com PHP', 'Carlos Menezes', 264, '978-4567890131', 75, 3.9),
        ('Viagem através do TypeScript', 'Jéssica Torres', 324, '978-4567890132', 130, 4.8),
        ('Histórias de Haskell', 'Felipe Araújo', 210, '978-4567890133', 60, 4.1),
        ('Crônicas de Clojure', 'Marina Lima', 278, '978-4567890134', 45, 3.7),
        ('Java e o Tempo', 'Henrique Faria', 350, '978-4567890135', 200, 4.3),
        ('Assembleia dos Algoritmos', 'Paula Gomes', 175, '978-4567890136', 170, 4.6),
        ('Segredos do Scala', 'Antônio Neto', 305, '978-4567890137', 90, 4.5);
END
```{{copy}}

E vamos alterar o docker-compose adicionando um comando para que esse script seja executado após 30 segundos:

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
      - ConnectionStrings__DefaultConnection=Server=database;Database=master;User=sa;Password=senha_qualquer#_1;Encrypt=false;

  db:
    container_name: database
    image: "mcr.microsoft.com/mssql/server:2022-latest"
    environment:
      SA_PASSWORD: "senha_qualquer#_1"
      ACCEPT_EULA: "Y"
    ports:
      - "1433:1433"
    volumes:
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    command:
      - /bin/bash
      - -c
      - |
        /opt/mssql/bin/sqlservr & sleep 30;
        /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P 'senha_qualquer#_1' -d master -i /docker-entrypoint-initdb.d/init.sql;
        wait
```{{copy}}

E então podemos executar novamente a composição de contêineres, primeiro com `docker-compose down` e depois com `docker-compose up`

Porta 8080: [Acesso ao WebAPI de Livros]({{TRAFFIC_HOST1_8080}}/api/Livros)
