## Buscando conteúdo com grep

O `grep` busca por texto **dentro** dos arquivos. Enquanto o `find` localiza arquivos pelo nome, o `grep` localiza arquivos pelo que eles contêm.

### Preparando o conteúdo

Vamos adicionar conteúdo aos arquivos criados no passo anterior:

`cat > ~/projeto/src/app.py << 'EOF'
import utils
import config

def iniciar():
    print("Iniciando aplicação...")
    config.carregar()

def parar():
    print("Encerrando aplicação...")
EOF`{{execute}}

`cat > ~/projeto/src/utils.py << 'EOF'
def formatar(texto):
    return texto.strip().lower()

def validar_email(email):
    return "@" in email
EOF`{{execute}}

`cat > ~/projeto/logs/app.log << 'EOF'
2024-01-15 10:00:01 INFO  Aplicação iniciada
2024-01-15 10:05:23 ERROR Falha ao conectar ao banco de dados
2024-01-15 10:05:30 INFO  Tentando reconectar...
2024-01-15 10:05:35 INFO  Conexão restabelecida
2024-01-15 10:10:00 ERROR Timeout na requisição /api/usuarios
EOF`{{execute}}

### Buscando uma palavra em um arquivo

`grep "ERROR" ~/projeto/logs/app.log`{{execute}}

`grep "INFO" ~/projeto/logs/app.log`{{execute}}

### Buscando em todos os arquivos de uma pasta

Com a flag `-r` (recursivo), o `grep` percorre todos os subdiretórios:

`grep -r "import" ~/projeto/src/`{{execute}}

### Opções mais usadas

`grep -n "ERROR" ~/projeto/logs/app.log`{{execute}}

`grep -i "error" ~/projeto/logs/app.log`{{execute}}

`grep -c "INFO" ~/projeto/logs/app.log`{{execute}}

`grep -v "INFO" ~/projeto/logs/app.log`{{execute}}

| Flag | Significado |
|---|---|
| `-n` | Mostra o número da linha de cada resultado |
| `-i` | Ignora maiúsculas/minúsculas |
| `-r` | Recursivo — busca em subdiretórios |
| `-c` | Conta o número de linhas que correspondem |
| `-v` | Inverte — mostra linhas que **não** correspondem |
| `-l` | Mostra apenas o nome dos arquivos que contêm o padrão |

### Mostrando contexto ao redor do resultado

`grep -A 2 "ERROR" ~/projeto/logs/app.log`{{execute}}

`grep -B 1 "ERROR" ~/projeto/logs/app.log`{{execute}}

| Flag | Significado |
|---|---|
| `-A N` | Mostra N linhas **depois** do resultado |
| `-B N` | Mostra N linhas **antes** do resultado |
| `-C N` | Mostra N linhas antes e depois |

### Buscando por padrões (expressões regulares)

O `grep` aceita expressões regulares, o que o torna muito poderoso:

`grep -E "ERROR|WARN" ~/projeto/logs/app.log`{{execute}}

`grep -E "^2024" ~/projeto/logs/app.log`{{execute}}

> `^` significa "começa com". `-E` habilita expressões regulares estendidas.
