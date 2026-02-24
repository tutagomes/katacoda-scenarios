## Redirecionamento e pipes

Uma das filosofias centrais do Linux é: **ferramentas simples que fazem uma coisa bem, combinadas para resolver problemas complexos**. Pipes e redirecionamento são o que torna isso possível.

### Redirecionando saída para arquivos

Todo comando produz saída para o terminal. Você pode redirecionar essa saída para um arquivo:

```bash
comando > arquivo.txt    # Cria ou sobrescreve o arquivo
comando >> arquivo.txt   # Adiciona ao final do arquivo (append)
```

`ls -la ~/projeto/ > ~/lista-projeto.txt`{{execute}}

`cat ~/lista-projeto.txt`{{execute}}

`echo "Nova linha adicionada" >> ~/lista-projeto.txt`{{execute}}

`cat ~/lista-projeto.txt`{{execute}}

### Redirecionando erros

Além da saída padrão (stdout), os comandos também produzem saída de erro (stderr):

```bash
comando 2> erros.txt      # Redireciona apenas erros
comando > saida.txt 2>&1  # Redireciona tudo para o mesmo arquivo
```

`ls /pasta-que-nao-existe 2> ~/erros.txt`{{execute}}

`cat ~/erros.txt`{{execute}}

### Pipes: conectando comandos

O pipe `|` passa a saída de um comando como entrada do próximo. Você pode encadear quantos quiser:

`ps aux | grep bash`{{execute}}

`ls -la ~/projeto/ | grep ".py"`{{execute}}

### Combinações práticas do dia a dia

**Contar linhas de resultado:**

`find ~/projeto -type f | wc -l`{{execute}}

**Ordenar a saída:**

`ls ~/projeto/logs/ | sort`{{execute}}

**Remover duplicatas:**

`echo -e "banana\nmaçã\nbanana\nlaranja" | sort | uniq`{{execute}}

**As 5 linhas mais recentes de um log:**

`tail -n 5 ~/projeto/logs/app.log`{{execute}}

**Filtrar e contar erros em logs:**

`grep "ERROR" ~/projeto/logs/app.log | wc -l`{{execute}}

**Ver os processos que mais consomem CPU:**

`ps aux --sort=-%cpu | head -5`{{execute}}

### tee: ver E salvar ao mesmo tempo

O `tee` exibe a saída no terminal e salva em arquivo simultaneamente:

`ls -la ~/projeto/ | tee ~/lista-projeto-2.txt`{{execute}}

`cat ~/lista-projeto-2.txt`{{execute}}

> Pipes e redirecionamento parecem simples, mas dominá-los transforma a forma como você trabalha no terminal. Com o tempo, você vai resolver em uma linha coisas que antes exigiam scripts inteiros.
