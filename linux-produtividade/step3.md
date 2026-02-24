## Gerenciamento de pacotes

O Ubuntu usa o **apt** (*Advanced Package Tool*) para instalar, atualizar e remover software. É como uma loja de aplicativos, mas pelo terminal.

### Atualizando a lista de pacotes

Antes de instalar qualquer coisa, atualize a lista de pacotes disponíveis nos repositórios:

`apt update`{{execute}}

> A lista de pacotes fica desatualizada com o tempo. `apt update` não instala nada — apenas atualiza o índice de versões disponíveis.

### Buscando pacotes

`apt search curl`{{execute}}

`apt show curl`{{execute}}

O `apt show` traz detalhes sobre o pacote: versão, tamanho, dependências e descrição.

### Instalando pacotes

`apt install -y curl`{{execute}}

`apt install -y jq`{{execute}}

A flag `-y` confirma automaticamente a instalação sem pedir confirmação interativa — essencial em scripts.

Verifique que foram instalados:

`curl --version`{{execute}}

`jq --version`{{execute}}

`which curl`{{execute}}

### Listando pacotes instalados

`apt list --installed 2>/dev/null | head -20`{{execute}}

Para verificar se um pacote específico está instalado:

`dpkg -l curl`{{execute}}

### Removendo pacotes

`apt remove -y jq`{{execute}}

`apt purge -y jq`{{execute}}

| Comando | Diferença |
|---|---|
| `apt remove` | Remove o pacote mas mantém arquivos de configuração |
| `apt purge` | Remove o pacote e todos os seus arquivos de configuração |

### Limpando pacotes desnecessários

Após instalar ou remover pacotes, dependências que não são mais necessárias ficam no sistema:

`apt autoremove -y`{{execute}}

`apt clean`{{execute}}

### Instalando múltiplos pacotes de uma vez

`apt install -y wget htop tree`{{execute}}

`tree ~`{{execute}}

O `tree` mostra a estrutura de diretórios de forma visual — muito útil para inspecionar projetos.

> No mundo Docker, o `apt` aparece constantemente nos Dockerfiles. Entender como ele funciona ajuda a criar imagens mais eficientes e a depurar problemas de instalação.
