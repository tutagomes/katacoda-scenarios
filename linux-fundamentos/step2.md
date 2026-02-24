## Navegando pelo sistema de arquivos

O Linux organiza todos os arquivos em uma única árvore de diretórios que começa na raiz `/`. Não há `C:\` ou `D:\` — tudo parte do mesmo lugar.

### A estrutura do Linux

```
/
├── etc/      → configurações do sistema
├── home/     → pastas pessoais dos usuários (/home/arthur)
├── var/      → dados variáveis: logs, cache, bancos de dados
├── tmp/      → arquivos temporários (apagados ao reiniciar)
├── usr/      → programas e bibliotecas instalados
└── root/     → pasta pessoal do usuário root (administrador)
```

`ls /`{{execute}}

### Mudando de diretório

`cd /etc`{{execute}}

`pwd`{{execute}}

`ls`{{execute}}

### Atalhos de navegação

```bash
cd ~        # vai para a pasta home do usuário atual
cd ..       # sobe um nível na hierarquia
cd -        # volta para o último diretório visitado
cd /        # vai para a raiz do sistema
```

`cd ~`{{execute}}

`cd /var/log`{{execute}}

`cd ..`{{execute}}

`pwd`{{execute}}

`cd -`{{execute}}

`pwd`{{execute}}

### Caminho absoluto vs relativo

Um **caminho absoluto** começa sempre em `/` e descreve o caminho completo:
```
/home/root/documentos/relatorio.txt
```

Um **caminho relativo** parte de onde você está agora:
```
documentos/relatorio.txt   (se você já está em /home/root)
../outro-usuario/arquivo   (sobe um nível e entra em outra pasta)
```

`cd /`{{execute}}

`ls usr/bin`{{execute}}

`cd ~`{{execute}}
