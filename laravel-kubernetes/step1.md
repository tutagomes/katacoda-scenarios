A ferramenta minikube já está instalada, vamos então verificar qual versão e iniciar o cluster local:



`minikube version`{{execute}}



Agora que vimos que está tudo certo, vamos iniciar o cluster:



`minikube start`{{execute}}



Pronto! A ferramenta já iniciou uma "máquina virtual" e um cluster Kubernetes lá dentro! Podemos então começar a analisar!



Para verificar o cluster, vamos recolher algumas informações com: **kubectl cluster-info**:



`kubectl cluster-info`{{execute}}



During this tutorial, we’ll be focusing on the command line for deploying and exploring our application.

E como é um cluster, podemos ver os nós conectados através do comando **kubectl get nodes**:



`kubectl get nodes`{{execute}}



Este comando em um cluster de alta performance deve retornar uma quantidade maior do que 1, mas como este é apenas um exemplo, faremos uso somente de um nó do nosso cluster. Note que o status do nó é *ready*, nos informando que já está configurado e conectado para hospedar nossa aplicação.