## Shell scripting básico

Um script shell é um arquivo de texto com comandos que o shell executa em sequência. É a forma mais rápida de automatizar tarefas repetitivas no Linux.

### Seu primeiro script

`cat > ~/saudacao.sh << 'EOF'
#!/bin/bash
echo "Olá, $USER!"
echo "Hoje é: $(date)"
echo "Você está em: $(pwd)"
EOF`{{execute}}

`chmod +x ~/saudacao.sh`{{execute}}

`~/saudacao.sh`{{execute}}

A primeira linha `#!/bin/bash` é chamada de **shebang** — ela diz ao sistema qual interpretador usar para executar o script.

### Variáveis em scripts

`cat > ~/info-sistema.sh << 'EOF'
#!/bin/bash
USUARIO=$(whoami)
DATA=$(date +"%d/%m/%Y")
HORA=$(date +"%H:%M")

echo "=== Informações do Sistema ==="
echo "Usuário: $USUARIO"
echo "Data: $DATA"
echo "Hora: $HORA"
echo "Uptime: $(uptime -p)"
EOF`{{execute}}

`chmod +x ~/info-sistema.sh && ~/info-sistema.sh`{{execute}}

O `$(comando)` executa o comando e captura sua saída como valor.

### Condicionais com if

`cat > ~/verificar-arquivo.sh << 'EOF'
#!/bin/bash
ARQUIVO="$1"

if [ -z "$ARQUIVO" ]; then
    echo "Uso: $0 <nome-do-arquivo>"
    exit 1
fi

if [ -f "$ARQUIVO" ]; then
    echo "Arquivo '$ARQUIVO' existe."
    echo "Tamanho: $(wc -c < "$ARQUIVO") bytes"
else
    echo "Arquivo '$ARQUIVO' não encontrado."
fi
EOF`{{execute}}

`chmod +x ~/verificar-arquivo.sh`{{execute}}

`~/verificar-arquivo.sh ~/saudacao.sh`{{execute}}

`~/verificar-arquivo.sh /arquivo-inexistente`{{execute}}

O `$1` é o primeiro argumento passado ao script. `$0` é o nome do script.

### Laços com for

`cat > ~/listar-extensoes.sh << 'EOF'
#!/bin/bash
PASTA="${1:-.}"

echo "Arquivos em '$PASTA':"
for ARQUIVO in "$PASTA"/*; do
    if [ -f "$ARQUIVO" ]; then
        NOME=$(basename "$ARQUIVO")
        TAMANHO=$(wc -c < "$ARQUIVO")
        echo "  $NOME ($TAMANHO bytes)"
    fi
done
EOF`{{execute}}

`chmod +x ~/listar-extensoes.sh`{{execute}}

`~/listar-extensoes.sh ~`{{execute}}

### Tratando erros com exit codes

Todo comando retorna um código de saída: `0` significa sucesso, qualquer outro valor indica erro.

`ls /tmp > /dev/null && echo "Funcionou!" || echo "Falhou!"`{{execute}}

`ls /pasta-inexistente > /dev/null && echo "Funcionou!" || echo "Falhou!"`{{execute}}

Em scripts, use `set -e` no início para interromper automaticamente se qualquer comando falhar — uma boa prática para evitar que erros passem silenciosamente:

`cat > ~/script-seguro.sh << 'EOF'
#!/bin/bash
set -e
echo "Iniciando..."
ls /tmp
echo "Tudo OK!"
EOF`{{execute}}

`chmod +x ~/script-seguro.sh && ~/script-seguro.sh`{{execute}}
