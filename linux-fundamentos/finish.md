## Parabéns!

Você deu seus primeiros passos no Linux e já sabe se virar no terminal!

### Resumo dos comandos aprendidos

**Orientação:**
```bash
whoami              # exibe o usuário atual
pwd                 # exibe o diretório atual
man <comando>       # abre o manual do comando
<comando> --help    # ajuda rápida do comando
clear               # limpa a tela (ou Ctrl+L)
```

**Navegação:**
```bash
ls                  # lista arquivos e pastas
ls -la              # formato detalhado incluindo ocultos
cd <caminho>        # muda de diretório
cd ~                # vai para a pasta home
cd ..               # sobe um nível
cd -                # volta para o último diretório
```

**Arquivos e pastas:**
```bash
touch <arquivo>          # cria arquivo vazio
mkdir <pasta>            # cria diretório
mkdir -p <a>/<b>/<c>     # cria hierarquia completa
cp <origem> <destino>    # copia arquivo
cp -r <origem> <destino> # copia diretório
mv <origem> <destino>    # move ou renomeia
rm <arquivo>             # remove arquivo
rm -r <pasta>            # remove diretório e conteúdo
rm -i <arquivo>          # remove com confirmação
```

**Leitura:**
```bash
cat <arquivo>            # exibe o arquivo inteiro
head -n 10 <arquivo>     # exibe as primeiras N linhas
tail -n 10 <arquivo>     # exibe as últimas N linhas
tail -f <arquivo>        # acompanha o arquivo em tempo real
less <arquivo>           # abre visualizador interativo
```

**nano:**
```
Ctrl+O  → salvar
Ctrl+X  → sair
Ctrl+W  → buscar
Ctrl+K  → recortar linha
Ctrl+U  → colar
```

### Próximo passo

Continue no cenário **"02 - Linux: Uso do dia a dia"** para aprender a buscar arquivos, gerenciar permissões, processos e encadear comandos!
