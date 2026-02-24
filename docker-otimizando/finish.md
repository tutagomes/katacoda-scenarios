## Parabéns!

Você agora sabe criar imagens Docker enxutas, eficientes e seguras — uma habilidade que faz diferença real no dia a dia de qualquer equipe!

### Resumo das técnicas aprendidas

**Escolha da imagem base:**

```dockerfile
FROM node:alpine        # ✅ ~50MB — use sempre que possível
FROM node:slim          # ⚠️ ~250MB — quando Alpine não for suficiente
FROM node:latest        # ❌ ~950MB — evite em produção
```

**Ordem das instruções (para melhor cache):**

```dockerfile
COPY package*.json ./        # 1. Copia o manifesto de dependências
RUN npm install --omit=dev   # 2. Instala (usa cache se package.json não mudou)
COPY . .                     # 3. Copia o código por último
```

**Multi-stage build:**

```dockerfile
FROM node:latest AS builder
# ... instala tudo, compila, gera artefatos

FROM node:alpine AS producao
COPY --from=builder /build/artefato .   # Copia só o necessário
```

**Comandos úteis:**

```bash
docker history <imagem>                      # Inspeciona camadas e tamanhos
docker build -f Dockerfile.prod -t app .     # Build com Dockerfile específico
docker build --target builder -t app .       # Build de um estágio específico
docker images                                # Compara tamanhos das imagens
```

**.dockerignore essencial:**

```
node_modules
.env
.env.*
*.log
.git
coverage/
tests/
```

### Próximo passo

Continue no cenário **"Docker: Publicando no Docker Hub"** para aprender a compartilhar suas imagens otimizadas com o mundo!
