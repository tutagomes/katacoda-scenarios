## Montando um volume

No step anterior, quando editamos o `server.js` e precisamos ver a mudança, tivemos que fazer um novo build e criar outro container. Para desenvolvimento, isso é lento demais. Existe um caminho melhor: **volumes**.

Um volume permite **compartilhar uma pasta da sua máquina com o container**. Assim, qualquer alteração que você fizer no código é refletida imediatamente dentro do container — sem precisar de rebuild.

### Preparando o ambiente

Primeiro, vamos parar e remover os containers anteriores para liberar a porta e o nome:

`docker stop minha-app && docker rm minha-app`{{execute}}

### Rodando com volume

Agora vamos rodar a imagem montando a pasta atual como volume dentro do container:

`docker run --name minha-app -d -p 9090:8080 -v ${PWD}:/usr/src/app minha-app:1.0`{{execute}}

Vamos entender a flag `-v`:

| Parte | Significado |
|---|---|
| `${PWD}` | O diretório atual da sua máquina (host) |
| `:` | Separador entre origem e destino |
| `/usr/src/app` | O diretório dentro do container (nosso WORKDIR) |

O que isso faz: o conteúdo de `/usr/src/app` dentro do container agora é **espelhado** com a pasta atual da sua máquina. Qualquer arquivo que você criar, editar ou apagar em um lado, aparece no outro.

### Testando na prática

Verifique que a aplicação está rodando:

`curl localhost:9090`{{execute}}

Agora, edite o `server.js` no editor ao lado e mude a mensagem `Olá, Docker!` para algo como `Olá, Volume!`. Salve o arquivo.

Como o Node.js não recarrega automaticamente, precisamos reiniciar o container para ele reler o arquivo:

`docker restart minha-app`{{execute}}

Teste novamente:

`curl localhost:9090`{{execute}}

A mensagem mudou — **sem rebuild da imagem!**

### Verificando o espelhamento

Vamos criar um arquivo na máquina host e verificar que ele aparece dentro do container:

`echo "arquivo criado no host" > teste-volume.txt`{{execute}}

`docker exec minha-app cat /usr/src/app/teste-volume.txt`{{execute}}

O contrário também funciona — criando dentro do container, aparece na sua máquina:

`docker exec minha-app sh -c "echo 'arquivo criado no container' > dentro.txt"`{{execute}}

`cat dentro.txt`{{execute}}

### Cuidado com o node_modules

Quando montamos a pasta inteira com `-v ${PWD}:/usr/src/app`, o conteúdo da máquina host **sobrescreve** o que estava no container — incluindo o `node_modules/` que foi instalado durante o build. Se sua máquina não tiver um `node_modules/` local, a aplicação vai quebrar.

Para contornar isso, uma técnica comum é usar um **volume anônimo** para proteger o `node_modules` do container:

```bash
docker run --name minha-app -d -p 9090:8080 \
  -v ${PWD}:/usr/src/app \
  -v /usr/src/app/node_modules \
  minha-app:1.0
```

O segundo `-v /usr/src/app/node_modules` (sem `:`) diz ao Docker: "mantenha o `node_modules` que já está no container — não sobrescreva com o do host".

### Limpando

Remova o arquivo de teste e o container:

`rm -f teste-volume.txt dentro.txt`{{execute}}

`docker stop minha-app && docker rm minha-app`{{execute}}

> **Resumo:** Volumes são essenciais para desenvolvimento local com Docker. Eles eliminam o ciclo de rebuild para cada alteração de código, mantendo o container sincronizado com seus arquivos.