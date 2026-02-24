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

Note que nosso container está rodando, mas a coluna **PORTS** está vazia — o container está isolado da sua máquina e ainda não é acessível externamente. Vamos resolver isso no próximo passo.
