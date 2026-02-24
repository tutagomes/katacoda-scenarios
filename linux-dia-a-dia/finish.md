## Parabéns!

Você agora domina as ferramentas que compõem o kit básico de qualquer desenvolvedor ou administrador Linux!

### Resumo dos comandos aprendidos

**Buscando arquivos:**

```bash
find ONDE -name "PADRÃO"     # Busca por nome
find ONDE -type f            # Apenas arquivos
find ONDE -type d            # Apenas diretórios
find ONDE -maxdepth N        # Limita profundidade
find ONDE -exec cmd {} \;    # Executa comando em cada resultado
```

**Buscando conteúdo:**

```bash
grep "texto" arquivo         # Busca em um arquivo
grep -r "texto" pasta/       # Busca recursiva
grep -n "texto" arquivo      # Mostra número de linha
grep -i "texto" arquivo      # Ignora maiúsculas/minúsculas
grep -v "texto" arquivo      # Linhas que NÃO contêm o texto
grep -c "texto" arquivo      # Conta ocorrências
grep -A 2 "texto" arquivo    # Mostra 2 linhas após o resultado
```

**Permissões:**

```bash
ls -la                       # Lista com permissões
chmod +x arquivo             # Adiciona permissão de execução
chmod 644 arquivo            # Arquivo normal (rw-r--r--)
chmod 755 script.sh          # Script executável (rwxr-xr-x)
chown usuario:grupo arquivo  # Muda dono e grupo
```

**Processos:**

```bash
ps aux                       # Lista todos os processos
ps aux | grep nome           # Filtra por nome
top                          # Monitor em tempo real
kill PID                     # Encerra processo (SIGTERM)
kill -9 PID                  # Força encerramento (SIGKILL)
pgrep nome                   # Encontra PID pelo nome
comando &                    # Roda em segundo plano
```

**Redirecionamento e pipes:**

```bash
cmd > arquivo.txt            # Redireciona saída (sobrescreve)
cmd >> arquivo.txt           # Redireciona saída (append)
cmd 2> erros.txt             # Redireciona apenas erros
cmd > saida.txt 2>&1         # Redireciona tudo
cmd1 | cmd2                  # Passa saída de cmd1 para cmd2
cmd | tee arquivo.txt        # Exibe e salva ao mesmo tempo
cmd | wc -l                  # Conta linhas do resultado
cmd | sort                   # Ordena resultado
cmd | sort | uniq            # Remove duplicatas
```

### Próximo passo

Continue para **"03 - Linux: Produtividade"** e aprenda a automatizar tarefas, gerenciar pacotes, variáveis de ambiente e agendar execuções com cron!
