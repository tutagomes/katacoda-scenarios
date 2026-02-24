## Permissões de arquivos

No Linux, cada arquivo e diretório tem um conjunto de **permissões** que define quem pode fazer o quê. Entender permissões é fundamental — especialmente quando scripts não executam ou arquivos não abrem.

### Lendo as permissões

`ls -la ~/projeto/src/`{{execute}}

A primeira coluna mostra as permissões. Por exemplo: `-rw-r--r--`

Ela tem 10 caracteres:

```
- rw- r-- r--
│  │   │   └── outros (o) — qualquer outro usuário
│  │   └─────── grupo (g) — usuários do mesmo grupo
│  └─────────── dono (u) — quem criou o arquivo
└────────────── tipo: - arquivo, d diretório, l link
```

Cada bloco de 3 caracteres representa: `r` (read/leitura), `w` (write/escrita), `x` (execute/execução). Um `-` significa que aquela permissão não está concedida.

### Tornando um arquivo executável

`cat > ~/projeto/src/executar.sh << 'EOF'
#!/bin/bash
echo "Script executado com sucesso!"
EOF`{{execute}}

Tente executar:

`~/projeto/src/executar.sh`{{execute}}

Vai falhar — o arquivo não tem permissão de execução. Vamos conceder:

`chmod +x ~/projeto/src/executar.sh`{{execute}}

`ls -la ~/projeto/src/executar.sh`{{execute}}

`~/projeto/src/executar.sh`{{execute}}

### O chmod em detalhe

O `chmod` aceita duas formas: **simbólica** (letras) e **numérica** (octal).

**Forma simbólica:**

```bash
chmod +x arquivo    # Adiciona execução para todos
chmod -w arquivo    # Remove escrita para todos
chmod u+x arquivo   # Adiciona execução só para o dono
chmod go-r arquivo  # Remove leitura de grupo e outros
```

**Forma numérica:**

Cada permissão tem um valor: `r=4`, `w=2`, `x=1`. Some os valores para cada grupo:

| Número | Permissões | Significado |
|---|---|---|
| `7` | rwx | Leitura, escrita e execução |
| `6` | rw- | Leitura e escrita |
| `5` | r-x | Leitura e execução |
| `4` | r-- | Apenas leitura |
| `0` | --- | Nenhuma permissão |

`chmod 644 ~/projeto/src/app.py`{{execute}}

`chmod 755 ~/projeto/src/executar.sh`{{execute}}

`ls -la ~/projeto/src/`{{execute}}

> `644` é o padrão para arquivos normais (dono lê e escreve, outros só leem). `755` é o padrão para scripts e executáveis.

### Alterando o dono com chown

O `chown` muda o usuário e/ou grupo dono de um arquivo:

```bash
chown usuario arquivo          # Muda o dono
chown usuario:grupo arquivo    # Muda dono e grupo
chown -R usuario pasta/        # Recursivo para toda a pasta
```

`ls -la ~/projeto/src/app.py`{{execute}}

> `chown` geralmente requer `sudo` para arquivos que não são seus.
