## O que é Docker?

Imagine que você desenvolveu uma aplicação que funciona perfeitamente no seu computador. Mas quando outra pessoa tenta rodar, aparece o famoso erro: *"funciona na minha máquina"*. Dependências diferentes, versões diferentes de bibliotecas, configurações diferentes do sistema operacional...

O Docker resolve esse problema empacotando a aplicação junto com **tudo que ela precisa para funcionar**: sistema operacional base, bibliotecas, dependências e código. Esse pacote é chamado de **imagem**.

### Imagem vs Container

Pense em uma **imagem** como a receita de um bolo: ela descreve todos os ingredientes e o modo de preparo, mas ainda não é o bolo em si.

Um **container** é o bolo pronto — uma instância em execução daquela imagem. E assim como você pode assar vários bolos com a mesma receita, pode criar quantos containers quiser a partir da mesma imagem.

### O que você vai aprender neste cenário

- Buscar imagens disponíveis no Docker Hub
- Baixar e listar imagens na sua máquina
- Executar containers com diferentes configurações
- Mapear portas entre o container e sua máquina
- Gerenciar o ciclo de vida dos containers (criar, parar, remover)
- Explorar o interior de um container em execução
