Vamos então partir para a execução!

Para começar, vamos executar apenas uma imagem, só para provar que está tudo certo! Aqui, precisamos informar o nome completo da imagem, assim como o repositório (caso esteja fora do Docker Hub).

Como exemplo, vamos executar uma imagem simples do Laravel

`kubectl create deployment exemplo-laravel --image=registry.gitlab.com/arthurgomesfaria/laravelcompleto:master`{{execute}}



O resultado do comando deve ser:

```bash
deployment.apps/exemplo-laravel created
```



Excelente! O que o comando de deploy faz?

- Procura por um nó disponível para execução da instância da aplicação
- Configura a aplicação para ser executada naquele nó
- Configura o cluster para que seja possível fazer o deploy novamente caso outros nós sejam conectados.



Vamos ver se está tudo certo?

`kubectl get deployments`{{execute}}

O resultado deve ser:

```bash
controlplane $ kubectl get deployments
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
exemplo-laravel       0/1     1            0           19s
```



E claro, podemos também ver qual o status de cada um dos pods ( [O que são pods?](https://kubernetes.io/docs/concepts/workloads/pods/pod/) )  com o comando:

`kubectl get pods`{{execute}}



E caso queira saber mais informações sobre cada um dos pods:

`kubectl describe pods`{{execute}}