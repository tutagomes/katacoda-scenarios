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

`docker exec container-a wget -q -O- http://container-b 2>&1 || echo "Falhou: nome não resolvido"`{{execute}}

O nome `container-b` não é resolvido. Seria necessário descobrir o IP manualmente e usá-lo diretamente — o que é frágil e trabalhoso.

Esse é exatamente o problema que as redes customizadas resolvem.

### A rede host

Com `--network host`, o container compartilha diretamente a rede da máquina — sem isolamento. Ele não precisa de `-p` para expor portas:

`docker run -d --name nginx-host --network host nginx`{{execute}}

A porta 80 do nginx já está acessível diretamente na máquina:

`curl -s localhost:80 | head -5`{{execute}}

Isso é mais rápido (sem tradução de portas), mas perde o isolamento — dois containers não podem usar a mesma porta. Em produção, o uso é raro.

`docker rm -f nginx-host`{{execute}}

### A rede none

Com `--network none`, o container fica completamente isolado — sem rede nenhuma:

`docker run --rm --network none alpine wget -q -T 2 -O- http://google.com 2>&1 || echo "Sem rede: como esperado"`{{execute}}

Nenhuma conexão de entrada ou saída. Útil para containers que processam apenas arquivos locais e não devem ter acesso à rede por segurança.

### Limpando

`docker rm -f container-a container-b`{{execute}}
