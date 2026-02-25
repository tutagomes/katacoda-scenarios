## Parabéns!

Você aplicou as principais práticas de segurança para containers Docker — do Dockerfile ao `docker run`.

### Checklist de segurança

Use como referência ao criar novos containers:

- [ ] **Usuário não-root** no Dockerfile (`USER`)
- [ ] **Filesystem read-only** (`--read-only` + `--tmpfs` onde necessário)
- [ ] **Limites de memória** (`--memory`)
- [ ] **Limites de CPU** (`--cpus`)
- [ ] **Limite de processos** (`--pids-limit`)
- [ ] **Capabilities removidas** (`--cap-drop ALL` + `--cap-add` apenas o necessário)

### Resumo dos comandos aprendidos

**No Dockerfile:**

```dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
RUN chown -R appuser:appgroup /usr/src/app
USER appuser
```

**No docker run:**

```bash
docker run -d \
  --read-only \                  # Filesystem somente leitura
  --tmpfs /tmp \                 # Escrita temporária em memória
  --memory 128m \                # Limite de RAM
  --cpus 0.5 \                   # Limite de CPU
  --pids-limit 50 \              # Máximo de processos
  --cap-drop ALL \               # Remove todas as capabilities
  --cap-add NET_BIND_SERVICE \   # Devolve apenas o necessário
  --user 1000:1000 \             # Força uid/gid (imagens de terceiros)
  -p 3000:3000 \
  minha-app
```

**Diagnóstico:**

```bash
docker exec <container> whoami           # Verifica o usuário do processo
docker exec <container> id               # Verifica uid/gid
docker inspect <container> --format='{{.HostConfig.Memory}}'    # Limite de RAM
docker inspect <container> --format='{{.State.OOMKilled}}'      # Se foi morto por OOM
```

### Princípio-chave

**Menor privilégio:** cada container deve ter apenas as permissões estritamente necessárias para funcionar. Tudo o mais deve ser explicitamente negado.

### Próximos passos sugeridos

- **Docker Compose** — aplique essas práticas em ambientes multi-container
- **Docker: Redes** — use isolamento de rede como camada adicional de segurança
