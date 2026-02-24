## O que é Docker?

Imagine que você desenvolveu uma aplicação que funciona perfeitamente no seu computador. Mas quando outra pessoa tenta rodar, aparece o famoso erro: *"funciona na minha máquina"*. Dependências diferentes, versões diferentes de bibliotecas, configurações diferentes do sistema operacional...

O Docker resolve esse problema empacotando a aplicação junto com **tudo que ela precisa para funcionar**: sistema operacional base, bibliotecas, dependências e código. Esse pacote é chamado de **imagem**.

### Imagem vs Container

Pense em uma **imagem** como um **pacote instalador**: um arquivo imutável que contém o sistema operacional base, as dependências e o software já configurado — tudo pronto para rodar. Uma imagem de nginx, por exemplo, pode ter dezenas de megabytes ou até gigabytes, dependendo do que inclui.

Um **container** é o processo em execução gerado a partir desse pacote. Você pode criar vários containers a partir da mesma imagem — cada um isolado, com seu próprio sistema de arquivos e sua própria rede —, assim como pode abrir vários processos a partir do mesmo executável.

### O que você vai aprender neste cenário

- Buscar imagens disponíveis no Docker Hub
- Baixar e listar imagens na sua máquina
- Executar containers com diferentes configurações
- Mapear portas entre o container e sua máquina
- Gerenciar o ciclo de vida dos containers (criar, parar, remover)
- Explorar o interior de um container em execução
