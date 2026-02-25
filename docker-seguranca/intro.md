## Segurança em Containers

Containers Docker oferecem isolamento, mas **isolamento não é segurança**. Por padrão, processos dentro de containers rodam como root, têm acesso de escrita ao filesystem inteiro e podem consumir todos os recursos da máquina. Se um atacante comprometer sua aplicação, ele herda todos esses privilégios.

Neste cenário, você vai aplicar práticas que reduzem drasticamente a superfície de ataque dos seus containers — cada uma com uma demonstração do risco e da solução.

### O que você vai aprender

- Entender o risco de rodar containers como root
- Criar e usar um usuário não-root no Dockerfile
- Proteger o filesystem do container com modo read-only
- Limitar memória, CPU e número de processos
- Remover capabilities desnecessárias do kernel

### O princípio por trás de tudo

**Menor privilégio** (*least privilege*): cada componente do sistema deve ter apenas as permissões estritamente necessárias para funcionar. Nada mais.

> **Pré-requisito:** Este cenário assume que você já sabe criar Dockerfiles e rodar containers. Se ainda não sabe, comece pelos cenários **"Docker: Executando Imagens"** e **"Docker: Criando sua primeira imagem"**.
