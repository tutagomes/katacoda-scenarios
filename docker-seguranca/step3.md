## Filesystem read-only

Mesmo com um usuário não-root, o container ainda permite escrita no filesystem. Um atacante poderia criar scripts maliciosos, modificar arquivos da aplicação ou armazenar ferramentas de ataque em pastas com permissão de escrita.

A flag `--read-only` monta o filesystem do container inteiro como **somente leitura**.

### Testando sem read-only

Primeiro, vamos ver o comportamento normal — o container consegue criar arquivos:

`docker run --rm app-segura sh -c "echo 'teste' > /tmp/arquivo.txt && cat /tmp/arquivo.txt"`{{execute}}

Funciona. Qualquer pasta com permissão de escrita para o usuário aceita novos arquivos.

### Ativando o modo read-only

`docker run -d --name app-readonly --read-only -p 3001:3000 app-segura`{{execute}}

Verifique que a aplicação funciona normalmente:

`curl localhost:3001`{{execute}}

Agora tente criar um arquivo:

`docker exec app-readonly sh -c "echo 'teste' > /tmp/arquivo.txt" 2>&1 || echo "Filesystem read-only: escrita bloqueada"`{{execute}}

O filesystem inteiro está protegido contra escrita — incluindo `/tmp`.

### Permitindo escrita em diretórios específicos com --tmpfs

Algumas aplicações precisam escrever em diretórios temporários (logs, cache, uploads). Para isso, use `--tmpfs` para criar pontos de escrita em memória:

`docker rm -f app-readonly`{{execute}}

`docker run -d --name app-readonly --read-only --tmpfs /tmp --tmpfs /var/log -p 3001:3000 app-segura`{{execute}}

Agora `/tmp` e `/var/log` aceitam escrita, mas tudo o mais continua protegido:

`docker exec app-readonly sh -c "echo 'funciona' > /tmp/arquivo.txt && cat /tmp/arquivo.txt"`{{execute}}

`docker exec app-readonly sh -c "echo 'teste' > /usr/src/app/hack.js" 2>&1 || echo "Fora do tmpfs: escrita bloqueada"`{{execute}}

O `--tmpfs` cria um filesystem temporário **em memória** — os dados desaparecem quando o container para. Isso é ideal para arquivos transitórios que não precisam persistir.

### Combinando com usuário não-root

A combinação de `--read-only` + `USER` + `--tmpfs` cria uma defesa em camadas:

```
1. USER appuser          → não é root, não pode escalar privilégios
2. --read-only           → não pode modificar o filesystem
3. --tmpfs /tmp          → escrita apenas em memória, em pastas específicas
```

Mesmo que um atacante ganhe execução de código, ele não consegue: instalar ferramentas, modificar a aplicação ou persistir qualquer alteração.

### Limpando

`docker rm -f app-readonly`{{execute}}

> **Resumo:** `--read-only` protege o filesystem contra modificações. Use `--tmpfs` para permitir escrita temporária apenas onde necessário.
