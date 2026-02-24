## Lendo arquivos

Existem várias formas de ler o conteúdo de um arquivo no terminal, cada uma adequada para um caso diferente.

### Criando um arquivo com conteúdo para praticar

`seq 1 50 | awk '{print "Linha " $1 ": conteúdo de exemplo no arquivo de log."}' > arquivo.txt`{{execute}}

`wc -l arquivo.txt`{{execute}}

Criamos um arquivo com 50 linhas. Agora vamos lê-lo de formas diferentes.

### cat — exibe o arquivo inteiro

`cat arquivo.txt`{{execute}}

Ideal para arquivos curtos. Para arquivos longos, o conteúdo passa rápido demais.

### head — exibe o início

`head arquivo.txt`{{execute}}

Por padrão mostra as primeiras 10 linhas. Para controlar:

`head -n 5 arquivo.txt`{{execute}}

### tail — exibe o final

`tail arquivo.txt`{{execute}}

`tail -n 5 arquivo.txt`{{execute}}

### tail -f — acompanha em tempo real

O `-f` (*follow*) é especialmente útil para monitorar arquivos de log enquanto uma aplicação roda — ele atualiza automaticamente quando novas linhas são adicionadas:

`tail -f arquivo.txt`{{execute}}

Pressione `Ctrl+C` para sair.

### less — leitura interativa

Para arquivos longos, o `less` é a melhor opção — ele abre um visualizador interativo:

`less arquivo.txt`{{execute}}

Dentro do `less`:
- `↑` `↓` ou `j` `k` — navegar linha a linha
- `Space` — avançar uma página
- `b` — voltar uma página
- `/texto` — buscar dentro do arquivo
- `q` — sair

### Lendo múltiplos arquivos de uma vez

`cat /etc/hostname /etc/os-release`{{execute}}
