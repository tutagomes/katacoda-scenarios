## Múltiplos Serviços com Docker Compose

No cenário anterior você aprendeu a sintaxe do Docker Compose com um único serviço. Mas o poder real do Compose aparece quando você precisa orquestrar **múltiplos serviços** que trabalham juntos.

Neste cenário vamos construir uma API .NET que registra e exibe o número de visitas — usando **Redis** como armazenamento em memória. Dois containers, uma única aplicação.

```
[browser]
    ↓ HTTP
[container: api — .NET 8]
    ↓ TCP 6379
[container: cache — Redis]
```

O Docker Compose vai:
- Subir os dois containers com um único comando
- Criar automaticamente uma rede para eles se comunicarem
- Garantir que o Redis suba antes da API

### O que você vai aprender

- Definir múltiplos serviços no `docker-compose.yml`
- Como containers se comunicam pelo nome do serviço
- Controlar a ordem de inicialização com `depends_on`
- Gerenciar logs e estado de serviços individualmente

> **Pré-requisito:** Complete o cenário **"Docker Compose: Introdução"** antes de continuar.
