## Rodando seu primeiro container

Agora que temos uma imagem baixada, vamos criar um container a partir dela:

`docker run -d --name meu-nginx nginx`{{execute}}

Vamos entender cada parte desse comando:

| Parte | Significado |
|---|---|
| `docker run` | Cria e inicia um novo container |
| `-d` | Modo *detached* — roda em segundo plano, liberando o terminal |
| `--name meu-nginx` | Define um nome para identificar o container |
| `nginx` | Nome da imagem a ser usada |

> Se você omitir `--name`, o Docker gera um nome aleatório (como `happy_torvalds`). Dar um nome explícito facilita muito o dia a dia.

### Verificando containers em execução

`docker ps`{{execute}}

As colunas mais importantes são:

| Coluna | Significado |
|---|---|
| **CONTAINER ID** | Identificador único (pode ser usado nos comandos no lugar do nome) |
| **IMAGE** | Imagem que originou o container |
| **STATUS** | Há quanto tempo está rodando |
| **PORTS** | Portas expostas |
| **NAMES** | Nome do container |

Note que a coluna **PORTS** mostra `80/tcp` — isso significa que o nginx anuncia internamente que escuta na porta 80 (declarado na imagem com a instrução `EXPOSE`).

Esse mapeamento existe apenas **dentro da máquina host**. Você pode acessar o container pelo seu IP interno:

`docker inspect meu-nginx -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'`{{execute}}

`curl http://$(docker inspect meu-nginx -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}')`{{execute}}

O nginx responde! Porém, esse acesso funciona **apenas de dentro da própria máquina**. De fora — outro computador, um navegador externo — a porta não está acessível. Para isso, precisamos mapear a porta explicitamente, o que veremos no próximo passo.
