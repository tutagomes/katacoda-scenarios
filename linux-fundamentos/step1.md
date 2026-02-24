## Primeiros passos no terminal

O terminal já está aberto à sua direita. Vamos começar com os comandos mais básicos.

### Quem sou eu e onde estou?

`whoami`{{execute}}

Exibe o nome do usuário atual.

`pwd`{{execute}}

**Print Working Directory** — mostra o caminho completo da pasta em que você está agora. O resultado `/root` significa que você está na pasta do usuário `root`.

### Listando o conteúdo da pasta

`ls`{{execute}}

Lista os arquivos e pastas do diretório atual. Parece vazio? Vamos adicionar alguns detalhes:

`ls -l`{{execute}}

A flag `-l` exibe o formato longo: permissões, dono, tamanho e data de modificação.

`ls -la`{{execute}}

A flag `-a` inclui arquivos ocultos — aqueles cujo nome começa com `.`. Note o `.bashrc` e outros arquivos de configuração.

### Limpando a tela

`clear`{{execute}}

Ou pressione `Ctrl+L` — atalho que funciona em qualquer terminal.

### O manual de qualquer comando

O Linux vem com documentação embutida para praticamente todos os comandos:

`man ls`{{execute}}

Use as setas para navegar e pressione `q` para sair.

Para uma ajuda mais rápida, a maioria dos comandos aceita `--help`:

`ls --help`{{execute}}

> **Dica:** Não tente memorizar tudo. O importante é saber que a documentação existe e como acessá-la.
