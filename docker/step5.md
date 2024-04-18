Vamos então executar!

`docker run --name myapp -d -p 8085:8080 cenario/tasks:latest`{{execute}}

O acesso externo está disponível nos links:

Porta 8085: [click here]({{TRAFFIC_HOST1_8085}}/index.html)

E claro, também é possível montar o volume na nossa imagem:

`docker run --name myappwithvolume -d -p 8086:8080 -v ${PWD}:/app bitnami/nginx:latest`{{execute}}

Note que no comando anterior, montamos a pasta local do site na pasta interna do contêiner. Então qualquer modificação também é refletida lá dentro. Por exemplo, podemos também adicionar um arquivo .html

`touch outro.html`{{execute}}

E vamos abrir o arquivo no editor ao lado e colar o conteúdo.

```html
<html>
    <body>
        <h1>
            Outro arquivo em HTML!
        </h1>
    </body>
</html>
```

E acessar o link, validando que nosso arquivo está de fato no servidor:

Porta 8086: [click here]({{TRAFFIC_HOST1_8086}}/outro.html)