## Rodando sua imagem

Com a imagem criada, agora é hora de executá-la como um container:

`docker run --name minha-app -d -p 9090:8080 minha-app:1.0`{{execute}}

Veja que usamos `-p 9090:8080` porque nossa aplicação escuta na porta `8080` dentro do container (conforme declaramos no `EXPOSE`), e estamos mapeando essa porta para a `9090` da nossa máquina.

Verifique se está rodando:

`docker ps`{{execute}}

### Acessando a aplicação

Teste via terminal:

`curl localhost:9090`{{execute}}

Ou acesse pelo browser: [Abrir na porta 9090]({{TRAFFIC_HOST1_9090}})

Você verá a mensagem que programamos no `server.js`, incluindo o **hostname do container** — que nada mais é do que o ID do container.

### Verificando os logs

Para ver o que a aplicação está registrando no console:

`docker logs minha-app`{{execute}}

Com a flag `-f` (*follow*), os logs ficam em tempo real, atualizando conforme novas requisições chegam:

`docker logs -f minha-app`{{execute}}

Faça outra requisição em outra aba e observe o log aparecer.

Pressione `Ctrl+C` para sair do modo follow.

### Testando o cache do build na prática

Agora vamos editar o `server.js` — mude a mensagem "Olá, Docker!" para algo diferente no editor ao lado, salve, e então faça um novo build:

`docker build -t minha-app:2.0 .`{{execute}}

Observe que os steps 1, 2, 3 e 4 (FROM, WORKDIR, COPY package e npm install) usam **cache** — apenas o step 5 (`COPY . .`) e os seguintes são reexecutados. O npm install não rodou de novo porque o `package.json` não mudou.
