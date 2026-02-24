## Dev vs Produção com Docker Compose

A mesma aplicação precisa se comportar de formas diferentes em cada ambiente:

| | Desenvolvimento | Produção |
|---|---|---|
| Código | Volume montado (hot reload) | Copiado na imagem |
| Logs | Detalhados (debug) | Apenas erros e warnings |
| Banco | Local, senha simples | Externo, senha segura |
| Ferramentas extras | Adminer, pgAdmin | Não |
| Restart | Não automático | `always` |

Criar um `docker-compose.yml` diferente para cada ambiente gera duplicação e divergências. A solução do Docker Compose é a **composição de arquivos**: um arquivo base com o que é comum, e arquivos de override com o que muda por ambiente.

```
docker-compose.yml          ← base (comum a todos)
docker-compose.override.yml ← dev (carregado automaticamente)
docker-compose.prod.yml     ← produção (passado com -f)
```

### O que você vai aprender

- Criar um arquivo Compose base com configuração compartilhada
- Usar `override` para sobrescrever configurações em desenvolvimento
- Separar configurações de produção em um arquivo dedicado
- Combinar arquivos com a flag `-f`
- Usar `profiles` para subir serviços opcionais

> **Pré-requisito:** Complete os cenários anteriores de Docker Compose antes de continuar.
