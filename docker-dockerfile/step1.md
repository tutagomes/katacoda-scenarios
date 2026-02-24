## Anatomia do Dockerfile

Um Dockerfile é um arquivo de texto simples, sempre chamado `Dockerfile` (sem extensão). Cada linha contém uma **instrução** que o Docker executa em sequência para montar a imagem.

Vamos conhecer as instruções mais importantes:

### FROM
Define a imagem base — o ponto de partida da sua imagem. Toda imagem começa a partir de outra.

```dockerfile
FROM node:alpine
```

Aqui estamos dizendo: "começa com a imagem oficial do Node.js na versão Alpine Linux" (uma distribuição minimalista, ideal para containers por ser muito leve).

### WORKDIR
Define o diretório de trabalho dentro do container. Todos os comandos seguintes serão executados a partir desse diretório.

```dockerfile
WORKDIR /usr/src/app
```

### COPY
Copia arquivos da sua máquina para dentro da imagem.

```dockerfile
COPY package*.json ./
COPY . .
```

### RUN
Executa um comando durante o processo de build da imagem. Normalmente usado para instalar dependências.

```dockerfile
RUN npm install
```

### EXPOSE
Documenta qual porta a aplicação vai usar dentro do container. É uma declaração de intenção — ela não publica a porta automaticamente, mas serve de guia para quem for rodar a imagem.

```dockerfile
EXPOSE 8080
```

### CMD
Define o comando padrão que será executado quando o container iniciar. Diferente do `RUN`, o `CMD` não é executado durante o build — só quando o container sobe.

```dockerfile
CMD ["node", "server.js"]
```

> **RUN vs CMD:** `RUN` acontece no build (uma vez, para preparar a imagem). `CMD` acontece no start (toda vez que o container é iniciado).

### ENTRYPOINT
Define o executável principal do container — o processo que vai rodar. Diferente do `CMD`, ele não pode ser substituído facilmente no `docker run`.

```dockerfile
ENTRYPOINT ["dotnet", "MinhaApi.dll"]
```

**CMD vs ENTRYPOINT — qual a diferença real?**

| | `CMD` | `ENTRYPOINT` |
|---|---|---|
| **Função** | Comando padrão — pode ser substituído | Executável fixo do container |
| **Substituível com `docker run`** | Sim — qualquer argumento extra substitui o CMD | Não — argumentos extras são passados como parâmetros |
| **Uso típico** | Scripts com comportamento variável | Binários que sempre devem rodar |

Exemplo prático:

```dockerfile
# Com CMD — posso trocar o comando ao rodar
CMD ["node", "server.js"]
# docker run minha-imagem node outro-script.js  ← funciona, substitui o CMD
```

```dockerfile
# Com ENTRYPOINT — o executável é fixo, apenas os argumentos variam
ENTRYPOINT ["dotnet"]
CMD ["MinhaApi.dll"]
# docker run minha-imagem OutroApp.dll  ← passa OutroApp.dll como argumento para dotnet
```

Quando usados juntos, `ENTRYPOINT` define o executável e `CMD` define os argumentos padrão. Essa combinação é a mais usada em imagens profissionais.

### HEALTHCHECK
Permite que o Docker verifique periodicamente se a aplicação dentro do container está funcionando corretamente — não apenas se o processo está rodando.

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD curl -f http://localhost:8080/saude || exit 1
```

Um container com `HEALTHCHECK` definido pode estar em três estados: `starting` (ainda aguardando a primeira verificação), `healthy` (verificação passou) ou `unhealthy` (verificação falhou). Esse status aparece no `docker ps` e é usado pelo Docker Compose para `depends_on: condition: service_healthy`.

### A ordem importa

O Docker constrói a imagem em camadas — cada instrução gera uma camada. E aqui há uma otimização importante: o Docker **reutiliza camadas do cache** quando nada mudou. Por isso, instruções que mudam com menos frequência (como instalar dependências) devem vir antes das que mudam com mais frequência (como copiar o código da aplicação).
