## Montando um volume

No step anterior, quando editamos o `server.js` e precisamos ver a mudança, tivemos que fazer um novo build e criar outro container. Para desenvolvimento, isso é lento demais. Existe um caminho melhor: **volumes**.

Um volume permite **compartilhar uma pasta da sua máquina com o container**. Assim, qualquer alteração que você fizer no código é refletida imediatamente dentro do container — sem precisar de rebuild.

### Preparando o ambiente

Primeiro, vamos parar e remover os containers anteriores para liberar a porta e o nome:

`docker stop minha-app && docker rm minha-app`{{execute}}

### A armadilha do volume simples

A primeira ideia seria montar a pasta atual diretamente no container:

```bash
docker run --name minha-app -d -p 9090:8080 \
  -v ${PWD}:/usr/src/app \
  minha-app:1.0
```

Vamos tentar:

`docker run --name minha-app -d -p 9090:8080 -v ${PWD}:/usr/src/app minha-app:1.0`{{execute}}

Verifique os logs:

`docker logs minha-app`{{execute}}

O container **não conseguiu iniciar**. O erro indica que o módulo `express` não foi encontrado. Mas nós instalamos ele no build com `RUN npm install` — o que aconteceu?

O problema é que o volume **sobrescreve completamente** a pasta `/usr/src/app` do container com o conteúdo da sua máquina. E na sua máquina não existe `node_modules/` — ele só existia dentro da imagem. Resultado: o `node_modules` sumiu e o Node.js não encontra o Express.

Vamos remover esse container:

`docker rm -f minha-app`{{execute}}

### A solução: proteger o node_modules

Para montar o código sem perder o `node_modules`, usamos um **volume anônimo** que preserva a pasta de dependências do container:

`docker run --name minha-app -d -p 9090:8080 -v ${PWD}:/usr/src/app -v /usr/src/app/node_modules minha-app:1.0`{{execute}}

Vamos entender as duas flags `-v`:

| Flag | Significado |
|---|---|
| `-v ${PWD}:/usr/src/app` | Monta a pasta atual da máquina no WORKDIR do container |
| `-v /usr/src/app/node_modules` | Cria um volume anônimo para `node_modules`, impedindo que seja sobrescrito |

O segundo `-v` (sem `:`) diz ao Docker: "mantenha o `node_modules` que já está na imagem — não sobrescreva com o do host".

Verifique que agora funciona:

`docker logs minha-app`{{execute}}

`curl localhost:9090`{{execute}}

### Testando na prática

Agora edite o `server.js` no editor ao lado e mude a mensagem `Olá, Docker!` para algo como `Olá, Volume!`. Salve o arquivo.

Como o Node.js não recarrega automaticamente, precisamos reiniciar o container para ele reler o arquivo:

`docker restart minha-app`{{execute}}

Teste novamente:

`curl localhost:9090`{{execute}}

A mensagem mudou — **sem rebuild da imagem!**

### Verificando o espelhamento

O volume funciona nos dois sentidos. Vamos criar um arquivo na máquina host e verificar que ele aparece dentro do container:

`echo "arquivo criado no host" > teste-volume.txt`{{execute}}

`docker exec minha-app cat /usr/src/app/teste-volume.txt`{{execute}}

O contrário também funciona — criando dentro do container, aparece na sua máquina:

`docker exec minha-app sh -c "echo 'arquivo criado no container' > dentro.txt"`{{execute}}

`cat dentro.txt`{{execute}}

### Limpando

Remova os arquivos de teste e o container:

`rm -f teste-volume.txt dentro.txt`{{execute}}

`docker stop minha-app && docker rm minha-app`{{execute}}

> **Resumo:** Volumes são essenciais para desenvolvimento local com Docker. Ao montar uma pasta, lembre-se de proteger o `node_modules` (ou qualquer pasta de dependências instalada durante o build) com um volume anônimo, senão o container não sobe.