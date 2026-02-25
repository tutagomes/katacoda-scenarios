## Removendo capabilities

O Linux divide os poderes do root em unidades menores chamadas **capabilities**. Por exemplo, `NET_BIND_SERVICE` permite escutar em portas abaixo de 1024, `SYS_ADMIN` permite montar filesystems, `NET_RAW` permite criar pacotes de rede brutos.

Por padrão, o Docker já remove algumas capabilities perigosas, mas ainda mantém várias que a maioria das aplicações não precisa.

### Vendo as capabilities padrão

`docker run --rm alpine sh -c "cat /proc/1/status | grep Cap"`{{execute}}

Os valores são bitmasks. Para ver os nomes legíveis, use `capsh`:

`docker run --rm alpine sh -c "apk add -q libcap && capsh --decode=\$(cat /proc/1/status | grep CapEff | awk '{print \$2}')"`{{execute}}

Você verá capabilities como `cap_chown`, `cap_kill`, `cap_net_raw`, `cap_setuid`, entre outras — muitas desnecessárias para uma aplicação web.

### O risco do NET_RAW

A capability `NET_RAW` permite criar pacotes de rede brutos — algo usado por ferramentas como `ping` e `tcpdump`, mas também por ataques de ARP spoofing e DNS spoofing dentro da rede Docker:

`docker run --rm alpine sh -c "apk add -q iputils && ping -c 1 google.com && echo 'NET_RAW: pacotes brutos permitidos'"`{{execute}}

Uma aplicação Node.js, Python ou Java nunca precisa de pacotes de rede brutos.

### Removendo todas as capabilities

A abordagem mais segura é remover **tudo** e devolver apenas o que for necessário:

`docker run --rm --cap-drop ALL alpine sh -c "apk add -q iputils && ping -c 1 google.com" 2>&1 || echo "Sem NET_RAW: ping bloqueado"`{{execute}}

Com `--cap-drop ALL`, o container perde todas as capabilities. O ping falha porque `NET_RAW` foi removida.

### Devolvendo apenas o necessário

Se sua aplicação precisa escutar em portas privilegiadas (abaixo de 1024), você devolve apenas essa capability:

`docker run --rm --cap-drop ALL --cap-add NET_BIND_SERVICE alpine sh -c "cat /proc/1/status | grep CapEff"`{{execute}}

Agora o container tem **apenas** `NET_BIND_SERVICE` — nenhuma outra capability. É o mínimo necessário.

### Aplicando na nossa aplicação

`docker run -d --name app-hardened --cap-drop ALL -p 3000:3000 app-segura`{{execute}}

`curl localhost:3000`{{execute}}

A aplicação funciona perfeitamente — ela nunca precisou de nenhuma capability especial. A maioria das aplicações web não precisa.

### O combo completo de segurança

Combinando tudo que aprendemos neste cenário:

```bash
docker run -d \
  --name app-producao \
  --read-only \
  --tmpfs /tmp \
  --memory 128m \
  --cpus 0.5 \
  --pids-limit 50 \
  --cap-drop ALL \
  -p 3000:3000 \
  app-segura
```

| Camada | Proteção |
|---|---|
| `USER appuser` (no Dockerfile) | Processo não é root |
| `--read-only` + `--tmpfs /tmp` | Filesystem protegido, escrita só em memória |
| `--memory 128m` | Limite de RAM |
| `--cpus 0.5` | Limite de CPU |
| `--pids-limit 50` | Proteção contra fork bomb |
| `--cap-drop ALL` | Zero capabilities desnecessárias |

### Limpando

`docker rm -f app-hardened`{{execute}}

> **Resumo:** `--cap-drop ALL` remove todas as capabilities do kernel. Devolva com `--cap-add` apenas as que sua aplicação realmente precisa. Para a maioria das aplicações web, nenhuma capability adicional é necessária.
