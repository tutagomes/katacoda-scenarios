## Variáveis de ambiente

Variáveis de ambiente são pares `NOME=valor` disponíveis para todos os processos em execução. Elas configuram o comportamento do sistema e das aplicações — sem precisar alterar código.

### Vendo as variáveis atuais

`env`{{execute}}

`env | head -20`{{execute}}

Existem centenas. As mais importantes:

| Variável | Significado |
|---|---|
| `HOME` | Diretório do usuário atual |
| `USER` | Nome do usuário atual |
| `PATH` | Lista de diretórios onde o shell busca comandos |
| `SHELL` | Shell em uso (`/bin/bash`, `/bin/zsh`, etc.) |
| `PWD` | Diretório atual |

### Acessando o valor de uma variável

`echo $HOME`{{execute}}

`echo $USER`{{execute}}

`echo $PATH`{{execute}}

O `$` é o que transforma o nome em valor. `PATH` é apenas texto; `$PATH` é o conteúdo da variável.

### Criando suas próprias variáveis

`NOME="Docker"
echo "Olá, $NOME!"`{{execute}}

`PORTA=8080
echo "Servindo na porta $PORTA"`{{execute}}

> Variáveis criadas assim existem apenas na sessão atual do terminal e não são visíveis para subprocessos.

### Exportando variáveis

Para que um processo filho (como um script) herde a variável, use `export`:

`export APP_ENV="producao"`{{execute}}

`export DB_HOST="localhost"`{{execute}}

`export DB_PORT="5432"`{{execute}}

`env | grep "APP_ENV\|DB_"`{{execute}}

### Tornando variáveis permanentes

Para que uma variável exista em todo novo terminal, adicione-a ao arquivo `~/.bashrc`:

`echo 'export MINHA_VAR="sempre-aqui"' >> ~/.bashrc`{{execute}}

`source ~/.bashrc`{{execute}}

`echo $MINHA_VAR`{{execute}}

O `source` aplica as mudanças sem precisar abrir um novo terminal.

### O PATH em detalhe

O `PATH` define onde o shell procura os executáveis quando você digita um comando:

`echo $PATH`{{execute}}

`which ls`{{execute}}

`which python3`{{execute}}

Para adicionar um diretório ao PATH (útil para scripts pessoais):

`mkdir -p ~/bin`{{execute}}

`export PATH="$HOME/bin:$PATH"`{{execute}}

`echo $PATH`{{execute}}

> Em aplicações Docker, variáveis de ambiente são a forma padrão de passar configuração para containers — senhas de banco, chaves de API, modo de execução. Você verá muito disso nos cenários de Docker.
