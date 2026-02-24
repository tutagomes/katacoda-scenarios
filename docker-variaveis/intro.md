## Variáveis de Ambiente

Imagine que você criou uma imagem Docker com a URL do banco de dados hardcoded no código. Para rodar em produção, você precisaria criar uma imagem diferente — ou pior, commitar credenciais no repositório.

Variáveis de ambiente resolvem isso de forma elegante: a **imagem continua a mesma**, mas o comportamento muda conforme o ambiente em que o container roda.

### O padrão 12-factor

O uso de variáveis de ambiente para configuração é um dos princípios do [12-factor app](https://12factor.net/pt_br/config), uma metodologia amplamente adotada para construir aplicações modernas. A ideia central é simples: **separe o código da configuração**.

```
mesma imagem  →  container de dev    (com DB_HOST=localhost)
              →  container de staging (com DB_HOST=staging.db)
              →  container de prod    (com DB_HOST=prod.db)
```

### O que você vai aprender neste cenário

- Passar variáveis diretamente pelo `docker run` com `-e`
- Usar um arquivo `.env` para organizar muitas variáveis
- Declarar valores padrão no Dockerfile com `ENV`
- Entender a diferença entre `ENV` (tempo de execução) e `ARG` (tempo de build)
- Configurar a mesma imagem para rodar em dev e produção

> **Pré-requisito:** Este cenário assume que você já sabe criar e rodar imagens Docker. Se ainda não sabe, comece pelos cenários **"Docker: Executando Imagens"** e **"Docker: Criando sua primeira imagem"**.
