## Inspecionando a imagem

Você construiu sua imagem, rodou containers, montou volumes. Mas como ver **o que tem dentro** de uma imagem? O Docker oferece ferramentas para inspecionar imagens sem precisar rodar um container.

### Listando as camadas com docker history

Cada instrução do Dockerfile gera uma camada. O comando `docker history` mostra todas elas:

`docker history minha-app:1.0`{{execute}}

A saída mostra, de baixo para cima:

| Coluna | Significado |
|---|---|
| `IMAGE` | ID da camada |
| `CREATED` | Quando foi criada |
| `CREATED BY` | A instrução do Dockerfile que gerou essa camada |
| `SIZE` | O tamanho que essa camada adicionou à imagem |

Perceba que a camada do `RUN npm install` é uma das maiores — ela contém todas as dependências baixadas. Já a camada do `EXPOSE` tem 0B, porque é apenas uma declaração de metadado.

Para ver os comandos completos (sem truncar):

`docker history --no-trunc minha-app:1.0`{{execute}}

### Inspecionando com docker inspect

O `docker inspect` retorna um JSON detalhado com **tudo** sobre a imagem:

`docker inspect minha-app:1.0`{{execute}}

A saída é extensa. Vamos filtrar as partes mais úteis usando `--format`:

**Variáveis de ambiente definidas na imagem:**

`docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' minha-app:1.0`{{execute}}

Você verá as variáveis do Node.js que vieram da imagem base, além de qualquer `ENV` que tenhamos adicionado.

**O comando padrão (CMD):**

`docker inspect --format='{{json .Config.Cmd}}' minha-app:1.0`{{execute}}

**A porta exposta:**

`docker inspect --format='{{json .Config.ExposedPorts}}' minha-app:1.0`{{execute}}

**O diretório de trabalho (WORKDIR):**

`docker inspect --format='{{.Config.WorkingDir}}' minha-app:1.0`{{execute}}

### Comparando tamanhos

Vamos comparar o tamanho da nossa imagem com a imagem base:

`docker images | grep -E "minha-app|node"`{{execute}}

A diferença entre a imagem `node:alpine` e a `minha-app:1.0` é exatamente o que o nosso Dockerfile adicionou: o código da aplicação e as dependências do npm.

### Adicionando metadados com LABEL

Uma boa prática é adicionar informações sobre a imagem diretamente no Dockerfile usando `LABEL`. Abra o `Dockerfile` no editor e adicione depois do `FROM`:

```dockerfile
FROM node:alpine

LABEL maintainer="seu@email.com"
LABEL version="1.0"
LABEL description="Aplicação Node.js de exemplo"

WORKDIR /usr/src/app
```{{copy}}

Faça o build com os labels:

`docker build -t minha-app:1.1 .`{{execute}}

Agora você pode consultar os labels:

`docker inspect --format='{{json .Config.Labels}}' minha-app:1.1`{{execute}}

Os labels não afetam o funcionamento da imagem — são apenas metadados. Mas são muito úteis em ambientes com muitas imagens, para saber quem mantém cada uma, qual a versão, etc.

### Limpando imagens

Com vários builds, você acumula imagens. Para ver todas:

`docker images`{{execute}}

Para remover uma imagem específica:

`docker rmi minha-app:1.1`{{execute}}

Para remover **todas as imagens que não estão sendo usadas** por nenhum container:

`docker image prune -a`{{execute}}

Confirme com `y` quando solicitado.

> **Resumo:** `docker history` mostra as camadas e seus tamanhos. `docker inspect` revela toda a configuração da imagem. `LABEL` adiciona metadados úteis. E `docker image prune` mantém seu ambiente limpo.