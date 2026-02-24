## Configurando ambientes diferentes

Agora vamos juntar tudo e simular um padrão muito comum no dia a dia: a **mesma imagem** rodando em desenvolvimento e produção com comportamentos diferentes, usando arquivos `.env` por ambiente.

### Criando os arquivos de ambiente

Arquivo para desenvolvimento:

<pre class="file" data-filename=".env.dev" data-target="replace">
APP_ENV=development
PORT=3000
MENSAGEM=Ambiente de Desenvolvimento - Pode fazer bagunça!
</pre>

Arquivo para produção:

<pre class="file" data-filename=".env.prod" data-target="replace">
APP_ENV=production
PORT=3000
MENSAGEM=Bem-vindo! Sistema em produção.
</pre>

### Subindo os dois ambientes

Rodando em modo desenvolvimento:

`docker run --name app-dev -d -p 3000:3000 --env-file .env.dev app-variaveis`{{execute}}

Rodando em modo produção (na porta 3001):

`docker run --name app-prod -d -p 3001:3000 --env-file .env.prod app-variaveis`{{execute}}

Acesse os dois e compare:

- [Ambiente de Dev - porta 3000]({{TRAFFIC_HOST1_3000}})
- [Ambiente de Prod - porta 3001]({{TRAFFIC_HOST1_3001}})

### Verificando as variáveis de cada container

`docker exec app-dev env | grep -E "APP_ENV|MENSAGEM"`{{execute}}

`docker exec app-prod env | grep -E "APP_ENV|MENSAGEM"`{{execute}}

### Conclusão

Com essa abordagem você tem:

- Uma única imagem Docker para todos os ambientes
- Configurações separadas por arquivo `.env.*`
- Nenhuma credencial ou configuração dentro da imagem
- Fácil troca de comportamento sem rebuild

Esse é o padrão recomendado para qualquer aplicação em container.
