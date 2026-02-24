## Agendamento com cron

O **cron** é o agendador de tarefas do Linux. Com ele você define que um script ou comando seja executado automaticamente em horários específicos — sem precisar fazer nada manualmente.

### Verificando se o cron está rodando

`service cron start`{{execute}}

`service cron status`{{execute}}

### A sintaxe do cron

Cada linha no crontab representa uma tarefa agendada, com o formato:

```
* * * * * comando-a-executar
│ │ │ │ │
│ │ │ │ └── dia da semana (0-7, onde 0 e 7 = domingo)
│ │ │ └──── mês (1-12)
│ │ └────── dia do mês (1-31)
│ └──────── hora (0-23)
└────────── minuto (0-59)
```

Exemplos:

| Expressão | Quando executa |
|---|---|
| `* * * * *` | Todo minuto |
| `0 * * * *` | A cada hora (no minuto 0) |
| `0 9 * * *` | Todo dia às 9h |
| `0 9 * * 1` | Toda segunda-feira às 9h |
| `0 0 1 * *` | No primeiro dia de cada mês à meia-noite |
| `*/5 * * * *` | A cada 5 minutos |
| `0 9,18 * * 1-5` | Às 9h e 18h, de segunda a sexta |

### Criando um script para agendar

`cat > ~/monitorar.sh << 'EOF'
#!/bin/bash
DATA=$(date +"%Y-%m-%d %H:%M:%S")
USO_DISCO=$(df -h / | tail -1 | awk '{print $5}')
echo "$DATA - Uso do disco: $USO_DISCO" >> /tmp/monitoramento.log
EOF`{{execute}}

`chmod +x ~/monitorar.sh`{{execute}}

`~/monitorar.sh`{{execute}}

`cat /tmp/monitoramento.log`{{execute}}

### Editando o crontab

O `crontab -e` abre o arquivo de agendamentos do seu usuário:

`crontab -l 2>/dev/null || echo "Nenhuma tarefa agendada ainda"`{{execute}}

Para adicionar uma tarefa que roda a cada minuto (útil para testar):

`echo "* * * * * $HOME/monitorar.sh" | crontab -`{{execute}}

`crontab -l`{{execute}}

Aguarde um minuto e verifique se a tarefa foi executada:

`sleep 65 && cat /tmp/monitoramento.log`{{execute}}

### Removendo uma entrada do crontab

Para editar ou remover tarefas, edite o crontab e apague a linha correspondente. Para remover tudo:

`crontab -r`{{execute}}

`crontab -l 2>/dev/null || echo "Crontab limpo"`{{execute}}

### Logs do cron

O cron registra sua atividade nos logs do sistema:

`grep CRON /var/log/syslog 2>/dev/null | tail -10 || echo "Log disponível em /var/log/cron"`{{execute}}

### Variáveis de ambiente no cron

O cron roda com um ambiente mínimo — sem as variáveis do seu terminal. Se o script depende de variáveis como `PATH`, defina-as explicitamente no crontab:

```
PATH=/usr/local/bin:/usr/bin:/bin
* * * * * /home/usuario/monitorar.sh
```

> Uma alternativa ao cron para tarefas mais complexas em sistemas modernos é o **systemd timers** — mas o cron ainda é a opção mais universal e simples para a maioria dos casos.
