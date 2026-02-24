## Docker Hub e Imagens

O **Docker Hub** ([hub.docker.com](https://hub.docker.com)) é o repositório público oficial de imagens Docker. Qualquer pessoa pode publicar imagens lá, e você pode usar imagens prontas de praticamente qualquer tecnologia — nginx, MySQL, Node.js, Python, Redis, e muito mais.

### Buscando imagens

Para buscar imagens disponíveis diretamente pelo terminal:

`docker search nginx`{{execute}}

No resultado, preste atenção nas colunas:

| Coluna | Significado |
|---|---|
| **NAME** | Nome da imagem |
| **STARS** | Popularidade — quanto maior, mais confiável |
| **OFFICIAL** | `[OK]` indica imagem mantida oficialmente pelo projeto |

> Sempre prefira imagens marcadas como **OFFICIAL** quando existirem. Elas são revisadas e atualizadas regularmente pela equipe responsável.

### Baixando uma imagem

Para baixar a imagem oficial do nginx:

`docker pull nginx`{{execute}}

Perceba que o Docker baixa a imagem em **camadas** (*layers*). Cada camada é uma parte da imagem, e elas são reutilizadas entre imagens diferentes. Isso significa que, se duas imagens compartilham uma camada, o Docker baixa aquela parte apenas uma vez — economizando tempo e espaço em disco.

### Listando imagens baixadas

`docker images`{{execute}}

As colunas informam:

| Coluna | Significado |
|---|---|
| **REPOSITORY** | Nome da imagem |
| **TAG** | Versão da imagem (`latest` = mais recente) |
| **IMAGE ID** | Identificador único |
| **SIZE** | Espaço ocupado no disco |

A tag `latest` é aplicada automaticamente quando nenhuma versão é especificada. Para baixar uma versão específica, use `docker pull nginx:1.25`, por exemplo.
