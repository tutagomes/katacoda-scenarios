## Passando variáveis com -e

A forma mais direta de passar uma variável de ambiente para um container é usando a flag `-e` no `docker run`. A variável fica disponível para a aplicação como se tivesse sido definida no sistema operacional.

### Experimentando com uma imagem pronta

A imagem oficial do nginx não usa variáveis de ambiente, então vamos usar a imagem `alpine` — uma distribuição Linux minimalista — para experimentar:

`docker run --rm alpine env`{{execute}}

O comando `env` lista todas as variáveis de ambiente do container. Note as que o Docker injeta automaticamente (como `PATH` e `HOME`).

Agora vamos passar uma variável personalizada:

`docker run --rm -e MEU_NOME=Arthur alpine env`{{execute}}

Veja que `MEU_NOME=Arthur` aparece na lista. Você também pode passar múltiplas variáveis:

`docker run --rm -e APP_ENV=producao -e APP_PORT=8080 -e DB_HOST=db.servidor.com alpine env`{{execute}}

> A flag `--rm` remove o container automaticamente após ele terminar — muito útil para testes rápidos que não precisam persistir.

### Acessando a variável dentro de um container interativo

`docker run --rm -it -e MENSAGEM="Olá do container!" alpine sh`{{execute}}

Agora dentro do container:

`echo $MENSAGEM`{{execute}}

`exit`{{execute}}

### Omitindo o valor

Se a variável já existe na sua máquina, você pode passar apenas o nome e o Docker usa o valor que está no ambiente local:

`export DB_PASSWORD=minha-senha-local`{{execute}}

`docker run --rm -e DB_PASSWORD alpine env | grep DB_PASSWORD`{{execute}}

Isso é útil em pipelines de CI/CD, onde as variáveis sensíveis já estão configuradas no ambiente.
