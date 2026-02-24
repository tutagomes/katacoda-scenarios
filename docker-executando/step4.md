## Ciclo de Vida do Container

Todo container passa por estados bem definidos ao longo da sua existência:

```
criado  →  rodando  →  parado  →  removido
```

### Vendo todos os containers (incluindo parados)

Até agora, `docker ps` mostra apenas containers **em execução**. Para ver todos, use a flag `-a`:

`docker ps -a`{{execute}}

### Parando um container

O comando `stop` encerra o processo do container de forma segura (envia um sinal para que ele finalize tudo corretamente). O container para, mas **não é apagado** — ele ainda existe no disco no estado `Exited`.

`docker stop meu-nginx`{{execute}}

`docker ps -a`{{execute}}

Note que `meu-nginx` aparece com status `Exited`. Ele parou, mas ainda está lá e poderia ser reiniciado com `docker start meu-nginx`.

### Políticas de restart

Por padrão, quando um container para — seja por falha ou por ter concluído sua tarefa — o Docker não faz nada. Mas você pode instruí-lo a reiniciar automaticamente usando a flag `--restart` no `docker run`:

`docker run -d --name nginx-sempre --restart always nginx`{{execute}}

Existem quatro políticas disponíveis:

| Política | Comportamento |
|---|---|
| `no` | Nunca reinicia (padrão) |
| `on-failure` | Reinicia apenas se o container sair com **código de erro** (≠ 0) |
| `always` | Reinicia sempre — inclusive se o Docker daemon for reiniciado |
| `unless-stopped` | Igual ao `always`, exceto se o container foi parado **manualmente** com `docker stop` |

**Quando usar cada uma:**

- **`no`** — desenvolvimento local. Você quer que falhas apareçam sem o Docker mascarar o problema reiniciando silenciosamente.
- **`on-failure`** — processos que podem falhar transitoriamente mas devem tentar de novo (ex: uma app aguardando um serviço externo subir).
- **`always`** — serviços que precisam estar sempre no ar, mesmo após o reboot do servidor.
- **`unless-stopped`** — o comportamento mais previsível para produção: reinicia automaticamente, mas respeita uma parada manual intencional.

Teste parando e reiniciando o container para ver a política em ação:

`docker stop nginx-sempre`{{execute}}

`docker ps -a`{{execute}}

`docker start nginx-sempre`{{execute}}

`docker rm -f nginx-sempre`{{execute}}

### Removendo containers

Para liberar o espaço e remover o container definitivamente:

`docker rm meu-nginx`{{execute}}

Você pode parar e remover múltiplos containers de uma só vez:

`docker stop nginx-web nginx-web2`{{execute}}

`docker rm nginx-web nginx-web2`{{execute}}

`docker ps -a`{{execute}}

> **Atalho útil:** A flag `-f` (force) para e remove o container em um único comando, sem precisar rodar `stop` antes:
> `docker rm -f nome-do-container`

### Removendo imagens

Para remover uma imagem baixada e liberar espaço em disco:

`docker rmi nginx`{{execute}}

> O Docker não deixa remover uma imagem enquanto existir algum container que a utilizou — mesmo que esteja parado. Remova os containers primeiro.
