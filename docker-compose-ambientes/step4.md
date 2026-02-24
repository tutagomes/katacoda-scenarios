## Combinando arquivos com -f

A flag `-f` permite especificar exatamente quais arquivos Compose usar — e em que ordem. Os arquivos são mesclados da esquerda para a direita, com os mais à direita sobrescrevendo os anteriores.

### Sintaxe

```bash
docker compose -f docker-compose.yml -f docker-compose.prod.yml <comando>
```

### Simulando o ambiente de produção localmente

Derrubar o ambiente de dev primeiro:

`docker compose down`{{execute}}

Agora subir com as configurações de produção (usando a senha do arquivo .env.prod):

`docker compose -f docker-compose.yml -f docker-compose.prod.yml --env-file .env.prod config`{{execute}}

O comando `config` mostra a configuração mesclada final — ideal para revisar antes de aplicar em produção.

### Verificando a diferença entre ambientes

Configuração de dev (automática):

`docker compose config | grep -A 3 "environment"`{{execute}}

Configuração de prod (explícita):

`docker compose -f docker-compose.yml -f docker-compose.prod.yml --env-file .env.prod config | grep -A 3 "environment"`{{execute}}

Veja que `ASPNETCORE_ENVIRONMENT` é `Development` em um e `Production` no outro — usando os mesmos arquivos base, apenas com overrides diferentes.

### Criando aliases para facilitar

Em projetos reais, é comum criar um `Makefile` ou scripts de shell para não precisar digitar os comandos longos:

`cat > Makefile << 'EOF'`{{execute}}
`dev:`{{execute}}
`	docker compose up -d --build`{{execute}}

`prod:`{{execute}}
`	docker compose -f docker-compose.yml -f docker-compose.prod.yml --env-file .env.prod up -d`{{execute}}

`down:`{{execute}}
`	docker compose down`{{execute}}
`EOF`{{execute}}

Assim o time usa simplesmente `make dev` ou `make prod`.
