## .dockerignore

Além das boas práticas no Dockerfile, existe outra otimização importante que acontece **antes** do build.

Quando você executa `docker build`, o Docker envia para o daemon todos os arquivos da pasta atual — o chamado **contexto de build**. Isso acontece mesmo que esses arquivos não sejam usados no Dockerfile.

Em projetos reais, essa pasta pode conter `node_modules` (centenas de MB), arquivos de log, credenciais, cache e código de testes. Tudo isso é transferido desnecessariamente.

O `.dockerignore` funciona exatamente como o `.gitignore`: lista padrões de arquivos e pastas que **não** devem ser incluídos no contexto de build.

### Simulando o problema

Vamos criar alguns arquivos que não deveriam ir para a imagem:

`mkdir -p node_modules && echo "simulando dependencias" > node_modules/fake.txt`{{execute}}

`echo "senha-super-secreta" > .env`{{execute}}

`echo "log de desenvolvimento" > debug.log`{{execute}}

`ls -la`{{execute}}

### Verificando que as variáveis e logs estão no contêiner

`docker build -t app-sem-dockerignore .`{{execute}}

`docker run --rm app-sem-dockerignore ls -la`{{execute}}

Veja que eles estão presentes. Vamos ver agora como removê-los automaticamente.

### Criando o .dockerignore

`touch .dockerignore`{{execute}}

```dockerignore
# Dependências (serão instaladas no build)
node_modules
npm-debug.log

# Variáveis de ambiente e segredos
.env
.env.*
*.pem
*.key

.ssh

# Logs
*.log
logs/

# Arquivos de desenvolvimento
.git
.gitignore
README.md
.vscode
.idea

# Arquivos de teste
*.test.js
test/
tests/
coverage/
```{{copy}}

### Verificando o impacto

Faça o build novamente e observe o contexto enviado:

`docker build -t app-com-dockerignore .`{{execute}}

### Confirmando que o .env não está na imagem

`docker run --rm app-com-dockerignore ls -la`{{execute}}

O arquivo `.env` não está lá — ele foi excluído do contexto antes mesmo de chegar ao Dockerfile. Isso é uma camada de proteção importante.
