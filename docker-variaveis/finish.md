## Parabéns!

Você agora sabe como configurar containers de forma flexível e segura usando variáveis de ambiente!

### Resumo dos comandos aprendidos

**No docker run:**

```bash
docker run -e VARIAVEL=valor <imagem>               # Passa uma variável
docker run -e VAR1=a -e VAR2=b <imagem>             # Passa múltiplas variáveis
docker run -e VARIAVEL <imagem>                     # Usa o valor do ambiente local
docker run --env-file .env <imagem>                 # Carrega variáveis de um arquivo
docker run --rm <imagem> env                        # Inspeciona variáveis do container
```

**No Dockerfile:**

```dockerfile
ENV PORTA=3000                  # Define valor padrão (disponível em tempo de execução)
ENV PORTA=3000 APP_ENV=dev      # Múltiplas variáveis em uma linha
ARG VERSAO=1.0                  # Disponível apenas durante o build
ENV VERSAO=$VERSAO              # Transfere ARG para ENV
EXPOSE $PORTA                   # Pode usar variáveis em outras instruções
```

**No docker build:**

```bash
docker build --build-arg VERSAO=2.0 -t app .       # Passa ARG durante o build
```

### Boas práticas

- Use `.env` para organizar variáveis localmente e adicione ao `.gitignore`
- Mantenha um `.env.example` no repositório com as chaves mas sem valores
- Nunca passe senhas via `ARG` — elas ficam visíveis no `docker history`
- Defina valores padrão seguros no Dockerfile com `ENV`

### Próximo passo

Continue no cenário **"Docker: Redes"** para aprender como fazer containers se comunicarem entre si!
