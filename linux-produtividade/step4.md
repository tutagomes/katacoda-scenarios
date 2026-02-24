## Rede e conectividade

Trabalhar no terminal frequentemente envolve testar conexões, fazer requisições HTTP e inspecionar interfaces de rede. Essas ferramentas são especialmente úteis ao depurar aplicações em containers.

### Verificando interfaces de rede

`ip addr`{{execute}}

`ip addr | grep "inet "`{{execute}}

O `ip addr` lista todas as interfaces de rede e seus endereços IP.

### Testando conectividade

`ping -c 4 google.com`{{execute}}

O `ping` verifica se um host está acessível. A flag `-c 4` limita a 4 tentativas.

### Verificando portas abertas

`ss -tlnp`{{execute}}

| Flag | Significado |
|---|---|
| `-t` | Apenas conexões TCP |
| `-l` | Apenas sockets em escuta (*listening*) |
| `-n` | Mostra números ao invés de nomes de serviço |
| `-p` | Mostra o processo dono de cada socket |

### Fazendo requisições HTTP com curl

O `curl` é uma ferramenta poderosa para fazer requisições HTTP pelo terminal:

`curl https://httpbin.org/get`{{execute}}

`curl -s https://httpbin.org/get | head -20`{{execute}}

Flags mais usadas:

| Flag | Significado |
|---|---|
| `-s` | Silencioso — sem barra de progresso |
| `-o arquivo` | Salva a resposta em um arquivo |
| `-L` | Segue redirecionamentos |
| `-I` | Mostra apenas os headers da resposta |
| `-X POST` | Define o método HTTP |
| `-H "Header: valor"` | Adiciona um header |
| `-d "dados"` | Envia dados no corpo da requisição |

`curl -I https://httpbin.org/get`{{execute}}

`curl -s https://httpbin.org/ip`{{execute}}

### Testando APIs com curl e jq

O `jq` formata e filtra JSON no terminal — muito útil com APIs:

`apt install -y jq 2>/dev/null; curl -s https://httpbin.org/get | jq .`{{execute}}

`curl -s https://httpbin.org/get | jq '.headers'`{{execute}}

`curl -s https://httpbin.org/get | jq '.origin'`{{execute}}

### Baixando arquivos com wget

O `wget` é especializado em baixar arquivos — mais simples que o `curl` para esse uso:

`wget -q https://httpbin.org/get -O /tmp/resposta.json`{{execute}}

`cat /tmp/resposta.json | jq .`{{execute}}

### Inspecionando DNS

`nslookup google.com`{{execute}}

`host google.com`{{execute}}

> Essas ferramentas são essenciais ao trabalhar com Docker: testar se um container consegue acessar outro, verificar se um serviço está escutando na porta correta, ou debugar problemas de rede entre containers.
