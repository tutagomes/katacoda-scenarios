## Redes no Docker

Por padrão, cada container Docker é um mundo isolado. Ele não enxerga outros containers e não é visto de fora — a menos que você configure isso explicitamente.

Mas aplicações reais raramente são compostas por um único container. Um sistema típico pode ter um container para a API, outro para o banco de dados e outro para o cache. Para que eles funcionem juntos, precisam se comunicar — e é aí que entram as **redes Docker**.

### Como funciona

O Docker gerencia redes virtuais que conectam containers entre si. Containers conectados à mesma rede conseguem se encontrar **pelo nome** — sem precisar saber IPs, que mudam a cada reinicialização.

```
[container: api]  ←──────────────→  [container: db]
       ambos na mesma rede "backend"
         api consegue acessar db:5432
```

### O que você vai aprender neste cenário

- Conhecer as redes padrão que o Docker cria automaticamente
- Criar redes customizadas para seus projetos
- Conectar containers a uma rede e fazê-los se comunicar pelo nome
- Entender como o isolamento de rede protege seus containers
- Inspecionar redes e diagnosticar problemas de conectividade

> **Pré-requisito:** Este cenário assume que você já sabe criar e rodar containers. Se ainda não sabe, comece pelo cenário **"Docker: Executando Imagens"**.
