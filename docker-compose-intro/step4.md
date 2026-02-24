## Comandos essenciais

Agora que a aplicação está definida no Compose, vamos conhecer os comandos do dia a dia.

### Subindo em modo detached

Na etapa anterior o Compose travou o terminal. Para rodar em segundo plano use `-d`:

`docker compose up -d`{{execute}}

O terminal fica livre e os containers rodam em background.

### Verificando o status

`docker compose ps`{{execute}}

Similar ao `docker ps`, mas mostra apenas os serviços definidos no `docker-compose.yml` atual. As colunas indicam nome, imagem, estado e portas.

### Acompanhando os logs

`docker compose logs`{{execute}}

Para acompanhar em tempo real (equivalente ao `docker logs -f`):

`docker compose logs -f`{{execute}}

Pressione `Ctrl+C` para sair do modo follow — os containers continuam rodando.

Para ver logs de um serviço específico (útil quando há múltiplos):

`docker compose logs api`{{execute}}

### Parando sem remover

`docker compose stop`{{execute}}

`docker compose ps`{{execute}}

Os containers existem mas estão parados. Para retomar:

`docker compose start`{{execute}}

### Parando e removendo tudo

`docker compose down`{{execute}}

Este é o comando mais completo: para os containers, remove-os e remove a rede criada pelo Compose. Os **volumes** são preservados por padrão.

Para remover também os volumes:

`docker compose down -v`{{execute}}

> **`stop` vs `down`:** Use `stop` quando quiser pausar temporariamente. Use `down` para limpar completamente o ambiente — útil no fim do dia ou antes de um rebuild.
