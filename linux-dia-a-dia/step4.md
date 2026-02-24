## Gerenciando processos

Um processo é qualquer programa em execução no sistema. Saber listar, monitorar e encerrar processos é essencial — especialmente quando algo está consumindo muitos recursos ou travou.

### Listando processos com ps

`ps`{{execute}}

O `ps` sem argumentos mostra apenas os processos da sua sessão atual. Para ver todos:

`ps aux`{{execute}}

| Coluna | Significado |
|---|---|
| **USER** | Usuário dono do processo |
| **PID** | Identificador único do processo |
| **%CPU** | Percentual de CPU sendo usado |
| **%MEM** | Percentual de memória sendo usado |
| **COMMAND** | Comando que iniciou o processo |

Para filtrar por nome, combine com `grep`:

`ps aux | grep bash`{{execute}}

### Monitorando em tempo real com top

O `top` é como o Gerenciador de Tarefas do Linux — atualiza automaticamente e mostra os processos que mais consomem recursos:

`top`{{execute}}

Para navegar:
- `q` — sair
- `k` — encerrar um processo (pedirá o PID)
- `M` — ordenar por uso de memória
- `P` — ordenar por uso de CPU

Pressione `q` para sair agora.

### Rodando processos em segundo plano

O `&` no final de um comando faz ele rodar em background, liberando o terminal:

`sleep 60 &`{{execute}}

`sleep 90 &`{{execute}}

`ps aux | grep sleep`{{execute}}

O número entre colchetes no output é o **job number** e o número seguinte é o **PID**.

### Encerrando processos com kill

O `kill` envia um sinal a um processo. O sinal padrão (`SIGTERM`, número 15) pede que o processo se encerre corretamente:

`kill $(pgrep sleep | head -1)`{{execute}}

`ps aux | grep sleep`{{execute}}

Se o processo não responder ao `SIGTERM`, use `SIGKILL` (número 9) — encerra forçosamente, sem chance de salvar estado:

`kill -9 $(pgrep sleep)`{{execute}}

`ps aux | grep sleep`{{execute}}

### Encontrando o PID pelo nome

`pgrep bash`{{execute}}

`pgrep -l bash`{{execute}}

O `pgrep` retorna o PID de processos pelo nome — muito útil para scripts de automação.

> Antes de usar `kill -9`, sempre tente `kill` simples primeiro. O `SIGKILL` não dá chance ao processo de liberar recursos, fechar arquivos ou gravar logs.
