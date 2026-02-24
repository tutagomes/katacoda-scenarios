## Parabéns!

Você agora entende como o Docker gerencia redes e como fazer containers se comunicarem com segurança e simplicidade!

### Resumo dos comandos aprendidos

**Gerenciamento de redes:**

```bash
docker network ls                          # Lista todas as redes
docker network create <nome>               # Cria uma nova rede
docker network rm <nome>                   # Remove uma rede
docker network prune                       # Remove redes sem containers
docker network inspect <nome>              # Exibe detalhes da rede
```

**Conectando containers:**

```bash
docker run --network <rede> ...            # Conecta na criação
docker network connect <rede> <container>  # Conecta um container existente
docker network connect --alias <apelido> <rede> <container>  # Com alias
docker network disconnect <rede> <container>  # Desconecta
```

**Diagnóstico:**

```bash
docker inspect <container> | grep -A 20 '"Networks"'   # Redes do container
docker exec <container> ping <outro-container>          # Testa conectividade
```

### Conceitos-chave

- Redes **customizadas** têm DNS interno — containers se encontram pelo nome
- A rede **bridge padrão** não tem DNS — só comunica por IP
- Containers em **redes diferentes** são isolados entre si por padrão
- Um container pode pertencer a **múltiplas redes** simultaneamente

### Próximo passo

Continue no cenário **"Docker: Otimizando Imagens"** para aprender a criar imagens menores e mais eficientes!
