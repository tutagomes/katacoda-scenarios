## Criando e manipulando arquivos

Agora vamos criar nossa própria estrutura de arquivos para praticar.

### Criando diretórios

`mkdir projetos`{{execute}}

`mkdir projetos/backend projetos/frontend`{{execute}}

A flag `-p` cria toda a hierarquia de uma vez, sem reclamar se já existir:

`mkdir -p projetos/backend/src/controllers`{{execute}}

`ls -R projetos`{{execute}}

### Criando arquivos

`touch projetos/backend/README.md`{{execute}}

`touch projetos/backend/src/index.js projetos/frontend/index.html`{{execute}}

`ls -R projetos`{{execute}}

### Copiando

```bash
cp origem destino          # copia arquivo
cp -r origem destino       # copia diretório inteiro (-r = recursivo)
```

`cp projetos/backend/README.md projetos/frontend/README.md`{{execute}}

`cp -r projetos/backend projetos/backend-backup`{{execute}}

### Movendo e renomeando

`mv` serve tanto para mover quanto para renomear — é a mesma operação:

`mv projetos/frontend/index.html projetos/frontend/index_old.html`{{execute}}

`mv projetos/backend-backup projetos/legado`{{execute}}

`ls projetos`{{execute}}

### Removendo

```bash
rm arquivo          # remove um arquivo
rm -r pasta         # remove pasta e todo seu conteúdo
rm -i arquivo       # pede confirmação antes de remover
```

`rm projetos/frontend/README.md`{{execute}}

`rm -r projetos/legado`{{execute}}

`ls projetos`{{execute}}

> **Atenção:** No Linux não existe "lixeira" no terminal. Um `rm` é permanente. Use `rm -i` quando tiver dúvidas.
