## O problema do root

Por padrão, o processo principal de um container Docker roda como **root** — o superusuário com permissão total sobre tudo.

### Verificando na prática

Vamos subir um container e conferir:

`docker run -d --name app-root nginx`{{execute}}

`docker exec app-root whoami`{{execute}}

`docker exec app-root id`{{execute}}

O resultado mostra `root` com `uid=0` — o mesmo nível de privilégio do administrador da máquina.

### Por que isso é um risco

Sendo root, o processo dentro do container pode fazer qualquer coisa no filesystem do container:

`docker exec app-root sh -c "echo 'invadido' > /etc/nginx/nginx.conf"`{{execute}}

`docker exec app-root sh -c "cat /etc/nginx/nginx.conf"`{{execute}}

O arquivo de configuração do nginx foi sobrescrito sem nenhuma resistência. Em uma aplicação real, se um atacante explorar uma vulnerabilidade (como uma injeção de código), ele teria esse mesmo nível de acesso: poderia ler segredos, modificar configurações, instalar ferramentas de ataque ou até tentar escapar do container.

### Instalando pacotes como root

O root também pode instalar ferramentas que facilitam a vida de um atacante:

`docker exec app-root sh -c "apt-get update -qq && apt-get install -y -qq curl > /dev/null 2>&1 && echo 'curl instalado com sucesso'"`{{execute}}

Uma aplicação web não deveria ter permissão para instalar pacotes. Mas como root, nada impede.

### Comparando com o host

O root dentro do container **não é** automaticamente root na máquina host (o Docker aplica isolamento via namespaces). Mas a camada de proteção é fina — vulnerabilidades de escape de container são descobertas periodicamente, e quando isso acontece, estar rodando como root dentro do container significa ser root fora dele também.

### Limpando

`docker rm -f app-root`{{execute}}

> **Resumo:** Rodar como root dentro do container é o padrão, mas é um risco desnecessário. No próximo step, vamos resolver isso.
