## Otimizando Imagens Docker

Você já sabe criar imagens Docker. Mas existe uma grande diferença entre uma imagem que *funciona* e uma imagem que é *boa de verdade*.

Imagens mal otimizadas podem ter centenas de megabytes a mais do que o necessário, incluir ferramentas de desenvolvimento em produção, expor arquivos sensíveis e tornar os builds mais lentos.

### Por que otimização importa

| Problema | Consequência |
|---|---|
| Imagem muito grande | Deploy mais lento, mais custo de armazenamento e tráfego |
| Muitas camadas desnecessárias | Build mais lento, cache menos eficiente |
| Arquivos sensíveis no contexto | Risco de vazar `.env`, chaves e credenciais |
| Ferramentas de build em produção | Superfície de ataque maior, imagem inchada |

### O que você vai aprender neste cenário

- Entender como as camadas afetam o tamanho da imagem
- Usar `.dockerignore` para excluir arquivos desnecessários do build
- Aplicar boas práticas na ordem e combinação de instruções
- Usar **multi-stage builds** para separar o ambiente de build do de execução
- Comparar o tamanho antes e depois das otimizações

> **Pré-requisito:** Este cenário assume que você já sabe escrever Dockerfiles. Se ainda não sabe, comece pelo cenário **"Docker: Criando sua primeira imagem"**.
