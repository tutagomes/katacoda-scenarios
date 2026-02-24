## Criando uma rede customizada

Redes customizadas do tipo `bridge` têm uma vantagem fundamental sobre a rede padrão: elas incluem um **servidor DNS interno** que permite que containers se encontrem pelo nome. Você nunca mais precisa gerenciar IPs manualmente.

### Criando a rede

`docker network create minha-rede`{{execute}}

Verifique que ela foi criada:

`docker network ls`{{execute}}

### Inspecionando a rede recém-criada

`docker network inspect minha-rede`{{execute}}

Note a faixa de IPs atribuída automaticamente (campo `"Subnet"`) e que ainda não há containers conectados.

### Subindo containers já conectados à rede

Use a flag `--network` para conectar um container à rede no momento da criação:

`docker run -d --name servidor --network minha-rede nginx`{{execute}}

`docker run -d --name cliente --network minha-rede alpine sleep 3600`{{execute}}

> O `sleep 3600` mantém o container `alpine` vivo por 1 hora sem fazer nada — útil para containers de teste que precisam ficar disponíveis para comandos manuais.

### Testando a resolução de nomes

Agora, dentro do container `cliente`, faça uma requisição HTTP ao `servidor` usando apenas seu nome:

`docker exec cliente wget -q -O- http://servidor`{{execute}}

Funcionou! O Docker resolveu automaticamente o nome `servidor` para o IP do container correto e a requisição chegou ao nginx. Isso é o DNS interno em ação — sem IPs hardcoded, sem configuração extra.

### Conectando um container existente a uma rede

Também é possível conectar um container que já está rodando a uma rede:

`docker run -d --name outro-servidor nginx`{{execute}}

`docker network connect minha-rede outro-servidor`{{execute}}

`docker exec cliente wget -q -O- http://outro-servidor`{{execute}}
