## Buscando arquivos com find

O `find` percorre o sistema de arquivos recursivamente e retorna os arquivos que correspondem aos critérios que você definir. É uma das ferramentas mais versáteis do Linux.

### Preparando os arquivos de exemplo

Vamos criar uma estrutura de pastas para praticar:

`mkdir -p ~/projeto/{src,docs,logs}`{{execute}}

`touch ~/projeto/src/app.py ~/projeto/src/utils.py ~/projeto/src/config.py`{{execute}}

`touch ~/projeto/docs/README.md ~/projeto/docs/API.md`{{execute}}

`touch ~/projeto/logs/app.log ~/projeto/logs/erro.log`{{execute}}

### Buscando por nome

A sintaxe básica é: `find ONDE -name "PADRÃO"`

`find ~/projeto -name "*.py"`{{execute}}

`find ~/projeto -name "*.log"`{{execute}}

`find ~/projeto -name "*.md"`{{execute}}

O `*` é um coringa que representa qualquer sequência de caracteres.

### Buscando por tipo

`find ~/projeto -type f`{{execute}}

`find ~/projeto -type d`{{execute}}

| Flag | Significado |
|---|---|
| `-type f` | Apenas arquivos |
| `-type d` | Apenas diretórios |

### Buscando apenas em um nível de profundidade

Por padrão, o `find` percorre todos os subdiretórios. Para limitar a profundidade:

`find ~/projeto -maxdepth 1 -type f`{{execute}}

`find ~/projeto -maxdepth 2 -name "*.py"`{{execute}}

### Combinando critérios

`find ~/projeto -name "*.log" -type f`{{execute}}

### Executando um comando em cada resultado

Com `-exec` você aplica um comando a cada arquivo encontrado:

`find ~/projeto -name "*.log" -exec ls -lh {} \;`{{execute}}

O `{}` é substituído pelo caminho de cada arquivo encontrado. O `\;` encerra o comando.

> O `find` é muito poderoso — mas para buscas por **conteúdo** dentro dos arquivos, a ferramenta certa é o `grep`, que veremos a seguir.
