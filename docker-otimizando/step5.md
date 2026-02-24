## Comparando os resultados

Chegou a hora de ver o impacto de todas as otimizações aplicadas.

### Listando todas as imagens que criamos

`docker images | grep app`{{execute}}

Você deve ver algo parecido com isso:

| Imagem | Tamanho aproximado |
|---|---|
| `app-sem-otimizacao` | ~950MB |
| `app-boas-praticas` | ~60MB |
| `app-multistage` | ~55MB |

A imagem final tem cerca de **94% menos** do que a original.

### Analisando as camadas da imagem otimizada

`docker history app-multistage`{{execute}}

Compare com a imagem original:

`docker history app-sem-otimizacao`{{execute}}

Note como a imagem otimizada tem muito menos camadas, e a maior delas é a imagem base Alpine — não mais o node padrão.

### Confirmando que a aplicação funciona igual

`docker run --rm app-sem-otimizacao node server.js &`{{execute}}

`docker run --rm app-multistage node server.js &`{{execute}}

Ambas funcionam da mesma forma, mas com tamanhos radicalmente diferentes.

### Resumo das técnicas aplicadas

| Técnica | Redução obtida |
|---|---|
| Trocar `node:latest` por `node:alpine` | ~85% |
| `.dockerignore` para excluir node_modules e .env | Contexto menor, build mais rápido |
| `npm install --omit=dev` | Remove dependências de desenvolvimento |
| Ordenar COPY para aproveitar o cache | Build mais rápido no dia a dia |
| Multi-stage build | Elimina ferramentas de build da imagem final |

### Uma imagem menor significa

- **Deploy mais rápido** — menos dados para transferir ao subir uma nova versão
- **Menos custos** — repositórios de imagem geralmente cobram por armazenamento
- **Mais seguro** — menos software instalado = menos vulnerabilidades em potencial
- **Startup mais rápido** — a imagem já está disponível localmente nos servidores
