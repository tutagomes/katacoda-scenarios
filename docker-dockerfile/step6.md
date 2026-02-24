## Montando um Volume

Até agora, o código da nossa aplicação está **dentro da imagem** — imutável. Para mudar qualquer coisa, precisaríamos reconstruir a imagem. Isso é ótimo para produção, mas pode ser trabalhoso durante o desenvolvimento.

Os **volumes** permitem mapear uma pasta da sua máquina diretamente para dentro do container. Qualquer mudança em um lado é imediatamente refletida no outro — sem precisar fazer um novo build.

A flag para isso é `-v`:

```
-v CAMINHO_NA_SUA_MAQUINA:CAMINHO_NO_CONTAINER
```

### Criando um arquivo de conteúdo estático

Vamos criar um arquivo HTML simples na pasta atual:

`mkdir -p public`{{execute}}

<pre class="file" data-filename="public/index.html" data-target="replace">
<!DOCTYPE html>
<html>
  <head><title>Volume Docker</title></head>
  <body>
    <h1>Este arquivo veio do volume!</h1>
    <p>Estou sendo servido diretamente da sua máquina, sem precisar de um novo build.</p>
  </body>
</html>
</pre>

### Rodando um container com volume

Vamos usar o nginx (uma imagem pronta) para servir esse arquivo, montando a pasta `public` diretamente no diretório que o nginx usa para arquivos estáticos:

`docker run --name nginx-volume -d -p 8082:80 -v ${PWD}/public:/usr/share/nginx/html nginx`{{execute}}

Acesse e veja o arquivo sendo servido: [Abrir na porta 8082]({{TRAFFIC_HOST1_8082}})

### A mágica do volume em ação

Agora edite o arquivo `public/index.html` no editor — mude o texto, salve, e **recarregue o browser** sem precisar reiniciar o container. A mudança aparece imediatamente!

### Quando usar volume vs imagem

| Situação | Abordagem |
|---|---|
| Desenvolvimento local | Volume — mudanças instantâneas sem rebuild |
| Banco de dados | Volume — persiste os dados mesmo se o container for removido |
| Deploy em produção | Imagem — tudo empacotado, garantia de reprodutibilidade |
| Configurações sensíveis | Volume — não ficam gravadas na imagem |

> **Importante:** Volumes **não fazem parte da imagem**. Eles existem separadamente no sistema de arquivos do host. Se você subir essa imagem para o Docker Hub, os dados do volume não vão junto — o que é justamente o comportamento desejado para bancos de dados e dados persistentes.
