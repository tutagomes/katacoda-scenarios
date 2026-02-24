## Provando a persistência

Agora vem a parte mais satisfatória: provar que os dados sobrevivem ao container ser destruído e recriado.

### Criando tarefas

`curl -s -X POST http://localhost:8080/tarefas \`{{execute}}
`  -H "Content-Type: application/json" \`{{execute}}
`  -d '{"titulo": "Aprender Docker Compose", "concluida": false}'`{{execute}}

`curl -s -X POST http://localhost:8080/tarefas \`{{execute}}
`  -H "Content-Type: application/json" \`{{execute}}
`  -d '{"titulo": "Criar cenários Killercoda", "concluida": true}'`{{execute}}

`curl -s -X POST http://localhost:8080/tarefas \`{{execute}}
`  -H "Content-Type: application/json" \`{{execute}}
`  -d '{"titulo": "Publicar no Docker Hub", "concluida": false}'`{{execute}}

### Confirmando que foram salvas

`curl -s http://localhost:8080/tarefas | python3 -m json.tool`{{execute}}

### Destruindo completamente os containers

`docker compose down`{{execute}}

`docker compose ps`{{execute}}

Containers removidos. Mas o volume ainda existe:

`docker volume ls`{{execute}}

### Subindo novamente do zero

`docker compose up -d`{{execute}}

`sleep 30`{{execute}}

### Os dados ainda estão lá!

`curl -s http://localhost:8080/tarefas | python3 -m json.tool`{{execute}}

As três tarefas persistiram mesmo após o container do SQL Server ser completamente destruído e recriado. O volume manteve os arquivos do banco intactos.

### Quando remover os volumes

Use `docker compose down -v` apenas quando quiser um reset completo — por exemplo, para testar do zero ou limpar o ambiente de desenvolvimento:

`docker compose down -v`{{execute}}

`docker volume ls`{{execute}}

O volume foi removido. Na próxima vez que subir, o banco estará vazio novamente.

### Executando comandos avulsos com docker compose run

Em aplicações reais com banco de dados, frequentemente precisamos executar comandos pontuais como migrações, seeds ou scripts de manutenção — sem deixar o container rodando como serviço. Para isso existe o `docker compose run`.

Diferente do `docker compose exec` (que executa em um container **já em execução**), o `docker compose run` **cria um novo container temporário** para rodar o comando e o remove ao terminar:

```bash
docker compose run --rm <serviço> <comando>
```

A flag `--rm` garante que o container temporário seja removido após a execução.

Suba o ambiente novamente para testar:

`docker compose up -d`{{execute}}

`sleep 30`{{execute}}

Vamos confirmar a versão do .NET dentro do container da API:

`docker compose run --rm api dotnet --version`{{execute}}

E listar as tarefas diretamente do container temporário:

`docker compose run --rm api sh -c "curl -s http://api:8080/tarefas"`{{execute}}

Em projetos .NET com EF Core, o padrão é usar `docker compose run` para rodar migrações sem precisar entrar no container manualmente:

```bash
# Aplicar migrações (exemplo com EF Core)
docker compose run --rm api dotnet ef database update

# Rodar seed de dados
docker compose run --rm api dotnet run --seed
```

**`docker compose run` vs `docker compose exec`:**

| | `docker compose run` | `docker compose exec` |
|---|---|---|
| **Container** | Cria um novo temporário | Usa um já em execução |
| **Porta exposta** | Não (por padrão) | Sim |
| **Uso típico** | Migrações, seeds, scripts únicos | Debug, inspeção, comandos pontuais |
| **`depends_on`** | Respeitado | Ignorado |
