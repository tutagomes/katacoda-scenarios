## Por que criar sua própria imagem?

No cenário anterior, você aprendeu a usar imagens prontas do Docker Hub. Mas e quando você tem uma **aplicação própria** que precisa ser empacotada e distribuída? É aí que entra o **Dockerfile**.

Um Dockerfile é um arquivo de texto com uma série de instruções que o Docker segue para construir uma imagem. É como uma receita detalhada que garante que sua aplicação será construída da mesma forma, em qualquer máquina, sempre.

### O ciclo completo

```
Código + Dockerfile  →  docker build  →  Imagem  →  docker run  →  Container
```

### O que você vai aprender neste cenário

- Conhecer as principais instruções de um Dockerfile
- Criar uma aplicação Node.js simples
- Escrever um Dockerfile para empacotá-la
- Fazer o build da imagem e entender o conceito de camadas (layers)
- Rodar e acessar sua imagem localmente
- Montar volumes para compartilhar arquivos entre sua máquina e o container

> **Pré-requisito:** Este cenário assume que você já conhece os comandos básicos do Docker (`docker run`, `docker ps`, `docker stop`). Se ainda não conhece, comece pelo cenário **"Docker: Executando Imagens"**.
