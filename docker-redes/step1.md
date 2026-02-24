## Redes padrão do Docker

Quando você instala o Docker, ele cria automaticamente três redes que ficam sempre disponíveis. Vamos conhecê-las:

`docker network ls`{{execute}}

Você verá as redes:

| Rede | Driver | Descrição |
|---|---|---|
| `bridge` | bridge | Rede padrão. Containers conectados podem se comunicar via IP, mas não por nome |
| `host` | host | O container compartilha diretamente a rede da máquina host |
| `none` | null | Container completamente isolado, sem nenhum acesso à rede |

### A rede bridge padrão

Quando você roda um container sem especificar rede, ele vai para a `bridge` automaticamente:

`docker run -d --name container-a alpine sleep 3600`{{execute}}

`docker run -d --name container-b alpine sleep 3600`{{execute}}

`docker network inspect bridge`{{execute}}

Role o output e veja a seção `"Containers"` — os dois containers aparecem lá, cada um com seu IP interno.

### O problema da bridge padrão

Containers na `bridge` padrão **não se enxergam pelo nome** — apenas por IP. E IPs mudam toda vez que um container é recriado. Teste isso:

`docker exec container-a ping -c 2 container-b`{{execute}}

Você vai receber um erro: o nome `container-b` não é resolvido. Seria necessário usar o IP diretamente, o que é frágil e trabalhoso.

Esse é exatamente o problema que as redes customizadas resolvem.

Vamos limpar antes de continuar:

`docker rm -f container-a container-b`{{execute}}
