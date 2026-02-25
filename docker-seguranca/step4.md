## Limitando recursos

Por padrão, um container pode consumir **toda** a memória e CPU disponíveis no host. Se uma aplicação tiver um memory leak ou for alvo de um ataque de negação de serviço (DoS), ela pode derrubar o servidor inteiro — inclusive outros containers.

### Limitando memória

Rode um container com limite de 64MB de memória:

`docker run -d --name app-limitada --memory 64m -p 3002:3000 app-segura`{{execute}}

Verifique o limite aplicado:

`docker inspect app-limitada --format='Memória: {{.HostConfig.Memory}}'`{{execute}}

O valor está em bytes (67108864 = 64MB).

### Simulando estouro de memória

Vamos criar um container que tenta alocar mais memória do que o permitido:

`docker run --rm --memory 32m alpine sh -c "dd if=/dev/zero of=/dev/null bs=64m count=1 & sleep 2 && cat /proc/meminfo | head -3"`{{execute}}

Com o limite de memória atingido, o kernel OOM killer encerra o processo. O container morre antes de afetar o restante do host.

Verifique se um container foi morto por OOM:

`docker run --name oom-test --memory 8m alpine sh -c "yes | tr -d '\n' | head -c 100m > /dev/null" 2>&1; docker inspect oom-test --format='OOMKilled: {{.State.OOMKilled}}'`{{execute}}

`docker rm -f oom-test`{{execute}}

### Limitando CPU

Limite o container a usar no máximo metade de um core da CPU:

`docker run -d --name app-cpu --cpus 0.5 -p 3003:3000 app-segura`{{execute}}

`docker inspect app-cpu --format='CPUs: {{.HostConfig.NanoCpus}}'`{{execute}}

O valor está em nanosegundos de CPU (500000000 = 0.5 CPUs).

### Limitando número de processos (fork bomb protection)

Uma **fork bomb** é um ataque simples onde um processo cria infinitos subprocessos até travar o sistema. O `--pids-limit` impede isso:

`docker run --rm --pids-limit 20 alpine sh -c "for i in \$(seq 1 30); do sleep 60 & done; echo 'Processos criados:'; ps | wc -l" 2>&1`{{execute}}

O container não conseguiu criar mais do que 20 processos — os excedentes falharam silenciosamente. Sem esse limite, um loop como esse poderia criar milhares de processos e travar o host.

### Combinando tudo em um docker run

Na prática, você aplica todas as restrições juntas:

```bash
docker run -d \
  --name app-produção \
  --memory 128m \
  --cpus 0.5 \
  --pids-limit 50 \
  -p 3000:3000 \
  app-segura
```

| Flag | Proteção |
|---|---|
| `--memory 128m` | Container não consome mais que 128MB de RAM |
| `--cpus 0.5` | Container usa no máximo metade de um core |
| `--pids-limit 50` | Máximo de 50 processos simultâneos |

### Limpando

`docker rm -f app-limitada app-cpu`{{execute}}

> **Resumo:** Limitar recursos impede que um container com problemas (ou sob ataque) derrube o host inteiro. Em produção, defina `--memory`, `--cpus` e `--pids-limit` para todos os containers.
