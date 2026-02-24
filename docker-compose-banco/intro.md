## Banco de Dados e Persistência

Containers são efêmeros por natureza: quando você faz `docker compose down`, tudo que estava dentro deles desaparece — incluindo os dados do banco. Isso é ótimo para ambientes descartáveis, mas um desastre para produção.

A solução são os **volumes nomeados**: áreas de armazenamento gerenciadas pelo Docker que existem independentemente dos containers. Os dados persistem mesmo que o container seja removido e recriado.

Neste cenário vamos construir uma API .NET de gerenciamento de tarefas conectada ao **SQL Server**, com um volume dedicado para os dados do banco.

```
[container: api — .NET 8 + EF Core]
         ↕ SQL (porta 1433)
[container: db — SQL Server 2022]
         ↕ volume
[volume: dados-sqlserver]  ← persiste no host
```

### O que você vai aprender

- Por que dados somem sem volumes e como evitar isso
- Configurar SQL Server como serviço no Compose
- Criar e usar volumes nomeados
- Aguardar o banco estar pronto antes de subir a API
- Verificar a persistência na prática

> **Pré-requisito:** Complete os cenários anteriores de Docker Compose antes de continuar.
