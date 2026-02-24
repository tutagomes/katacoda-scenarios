## Parabéns!

Você completou os três cenários de Linux e agora tem uma base sólida para trabalhar em ambientes Linux, containers Docker e servidores em geral!

### Resumo dos comandos aprendidos

**Variáveis de ambiente:**

```bash
env                          # Lista todas as variáveis
echo $VARIAVEL               # Exibe o valor de uma variável
export NOME="valor"          # Cria e exporta para subprocessos
echo 'export VAR="x"' >> ~/.bashrc  # Torna permanente
source ~/.bashrc             # Recarrega o .bashrc
which comando                # Mostra o caminho do executável
```

**Shell scripting:**

```bash
#!/bin/bash                  # Shebang — define o interpretador
set -e                       # Para o script se qualquer comando falhar
$1, $2, ...                  # Argumentos do script
$(comando)                   # Captura a saída de um comando
if [ condição ]; then ... fi # Condicional
for VAR in lista; do ... done # Laço
```

**Gerenciamento de pacotes:**

```bash
apt update                   # Atualiza lista de pacotes
apt install -y pacote        # Instala um pacote
apt remove -y pacote         # Remove (mantém configs)
apt purge -y pacote          # Remove completamente
apt autoremove -y            # Remove dependências órfãs
apt search termo             # Busca pacotes
dpkg -l pacote               # Verifica se está instalado
```

**Rede:**

```bash
ip addr                      # Lista interfaces e IPs
ping -c 4 host               # Testa conectividade
ss -tlnp                     # Portas em escuta
curl -s URL                  # Faz requisição HTTP
curl -I URL                  # Mostra apenas headers
curl -s URL | jq .           # Formata resposta JSON
wget -q URL -O arquivo       # Baixa arquivo
nslookup dominio             # Consulta DNS
```

**Cron:**

```bash
crontab -l                   # Lista tarefas agendadas
crontab -e                   # Edita tarefas agendadas
crontab -r                   # Remove todas as tarefas
# Formato: minuto hora dia-mês mês dia-semana comando
# Exemplo: 0 9 * * 1-5 /home/user/script.sh
```

### Próximos passos

Com essa base de Linux, você está pronto para aprofundar nos cenários de Docker e Docker Compose. Tudo que acontece dentro de um container é Linux — e agora você entende o que está acontecendo!

Continue pelo caminho:

1. **01 - Docker: Executando Imagens**
2. **02 - Docker: Criando sua primeira imagem**
3. **03 - Docker: Variáveis de Ambiente**
4. **04 - Docker: Redes**
5. **05 - Docker: Otimizando Imagens**
6. **01 - Docker Compose: Introdução** → e os demais cenários de Compose
