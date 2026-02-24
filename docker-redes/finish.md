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
docker run --network <rede> ...                        # Conecta na criação
docker run --network <rede> --network-alias <apelido>  # Com alias na criação
docker network connect <rede> <container>              # Conecta um container existente
docker network connect --alias <apelido> <rede> <container>  # Com alias
docker network disconnect <rede> <container>           # Desconecta
```

**Redes especiais:**

```bash
docker run --network host ...              # Compartilha a rede do host (sem -p)
docker run --network none ...              # Container sem acesso à rede
```

**Diagnóstico:**

```bash
docker inspect <container> --format='{{json .NetworkSettings.Networks}}'  # Redes do container
docker exec <container> wget -q -T 3 -O- http://<outro>                  # Testa conectividade
```

### Conceitos-chave

- Redes **customizadas** têm DNS interno — containers se encontram pelo nome
- A rede **bridge padrão** não tem DNS — só comunica por IP
- A rede **host** elimina o isolamento de rede — sem mapeamento de portas
- A rede **none** isola o container completamente — sem rede nenhuma
- Containers em **redes diferentes** são isolados entre si por padrão
- Um container pode pertencer a **múltiplas redes** simultaneamente

### Próximo passo

Continue no cenário **"Docker: Otimizando Imagens"** para aprender a criar imagens menores e mais eficientes!
