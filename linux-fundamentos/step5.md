## Editando com nano

O `nano` é o editor de texto mais acessível para quem está começando com Linux. Ele roda direto no terminal e mostra os atalhos na parte de baixo da tela — não precisa memorizar nada antes de começar.

### Abrindo um arquivo

`nano projetos/backend/README.md`{{execute}}

Você verá o editor abrir. Na parte inferior, os atalhos são mostrados com `^` representando a tecla `Ctrl`.

### Escrevendo conteúdo

Digite o seguinte texto (ou qualquer outro):

```
# Projeto Backend

Este é o README do projeto.

## Como rodar
1. Instale as dependências
2. Execute o servidor
```

### Atalhos essenciais do nano

| Atalho | Ação |
|---|---|
| `Ctrl+O` | Salvar o arquivo |
| `Enter` | Confirmar o nome do arquivo ao salvar |
| `Ctrl+X` | Sair (pergunta se quer salvar se houver mudanças) |
| `Ctrl+K` | Recortar a linha inteira |
| `Ctrl+U` | Colar |
| `Ctrl+W` | Buscar texto |
| `Ctrl+G` | Exibir ajuda completa |

Salve o arquivo com `Ctrl+O` → `Enter` e saia com `Ctrl+X`.

### Confirmando que foi salvo

`cat projetos/backend/README.md`{{execute}}

### Criando um novo arquivo diretamente

Você também pode passar um nome de arquivo que ainda não existe — o nano cria automaticamente ao salvar:

`nano projetos/frontend/index.html`{{execute}}

Digite algum HTML, salve e saia. Depois confirme:

`cat projetos/frontend/index.html`{{execute}}

> **Outros editores:** O `nano` é o mais amigável para iniciantes. Com o tempo você pode conhecer o `vim` (poderoso, curva de aprendizado alta) ou o `micro` (moderno e intuitivo). Para edições rápidas em servidores, `nano` resolve bem.
